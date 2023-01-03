# Using a credential provider<a name="guide_credentials_provider"></a>

A credential provider is a function that returns a `GuzzleHttp\Promise\PromiseInterface` that is fulfilled with an `Aws\Credentials\CredentialsInterface` instance or rejected with an `Aws\Exception\CredentialsException`\. You can use credential providers to implement your own custom logic for creating credentials or to optimize credential loading\.

Credential providers are passed into the `credentials` client constructor option\. Credential providers are asynchronous, which forces them to be lazily evaluated each time an API operation is invoked\. As such, passing in a credential provider function to an SDK client constructor doesn’t immediately validate the credentials\. If the credential provider doesn’t return a credentials object, an API operation will be rejected with an `Aws\Exception\CredentialsException`\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

// Use the default credential provider
$provider = CredentialProvider::defaultProvider();

// Pass the provider to the client
$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

## Built\-In Providers in the SDK<a name="built-in-providers-in-the-sdk"></a>

The SDK provides several built\-in providers that you can combine with any custom providers\. For more information on configuring the standardized providers and the credential provider chain in AWS SDKs, see [Standardized credential providers](https://docs.aws.amazon.com/sdkref/latest/guide/standardized-credentials.html) in the *AWS SDKs and Tools Reference Guide*\.

**Important**  
Credential providers are invoked every time an API operation is performed\. If loading credentials is an expensive task \(e\.g\., loading from disk or a network resource\), or if credentials are not cached by your provider, consider wrapping your credential provider in an `Aws\Credentials\CredentialProvider::memoize` function\. The default credential provider used by the SDK is automatically memoized\.

## assumeRole provider<a name="assumerole-provider"></a>

If you use `Aws\Credentials\AssumeRoleCredentialProvider` to create credentials by assuming a role, you need to provide `'client'` information with an `StsClient` object and `'assume_role_params'` details, as shown\.

**Note**  
To avoid unnecessarily fetching AWS STS credentials on every API operation, you can use the `memoize` function to handle automatically refreshing the credentials when they expire\. See the following code for an example\.

```
use Aws\Credentials\CredentialProvider;
use Aws\Credentials\InstanceProfileProvider;
use Aws\Credentials\AssumeRoleCredentialProvider;
use Aws\S3\S3Client;
use Aws\Sts\StsClient;

// Passing Aws\Credentials\AssumeRoleCredentialProvider options directly
$profile = new InstanceProfileProvider();
$ARN = "arn:aws:iam::123456789012:role/xaccounts3access";
$sessionName = "s3-access-example";

$assumeRoleCredentials = new AssumeRoleCredentialProvider([
    'client' => new StsClient([
        'region' => 'us-east-2',
        'version' => '2011-06-15',
        'credentials' => $profile
    ]),
    'assume_role_params' => [
        'RoleArn' => $ARN,
        'RoleSessionName' => $sessionName,
    ],
]);

// To avoid unnecessarily fetching STS credentials on every API operation,
// the memoize function handles automatically refreshing the credentials when they expire
$provider = CredentialProvider::memoize($assumeRoleCredentials);

$client = new S3Client([
    'region'      => 'us-east-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

For more information regarding `'assume_role_params'`, see [AssumeRole](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sts-2011-06-15.html#assumerole)\.

## Chaining providers<a name="chaining-providers"></a>

You can chain credential providers by using the `Aws\Credentials\CredentialProvider::chain()` function\. This function accepts a variadic number of arguments, each of which are credential provider functions\. This function then returns a new function that’s the composition of the provided functions, such that they are invoked one after the other until one of the providers returns a promise that is fulfilled successfully\.

The `defaultProvider` uses this composition to check multiple providers before failing\. The source of the `defaultProvider` demonstrates the use of the `chain` function\.

```
// This function returns a provider
public static function defaultProvider(array $config = [])
{
    // This function is the provider, which is actually the composition
    // of multiple providers. Notice that we are also memoizing the result by
    // default.
    return self::memoize(
        self::chain(
            self::env(),
            self::ini(),
            self::instanceProfile($config)
        )
    );
}
```

## Creating a custom provider<a name="creating-a-custom-provider"></a>

Credential providers are simply functions that when invoked return a promise \(`GuzzleHttp\Promise\PromiseInterface`\) that is fulfilled with an `Aws\Credentials\CredentialsInterface` object or rejected with an `Aws\Exception\CredentialsException`\.

A best practice for creating providers is to create a function that is invoked to create the actual credential provider\. As an example, here’s the source of the `env` provider \(slightly modified for example purposes\)\. Notice that it is a function that returns the actual provider function\. This allows you to easily compose credential providers and pass them around as values\.

```
use GuzzleHttp\Promise;
use GuzzleHttp\Promise\RejectedPromise;

// This function CREATES a credential provider
public static function env()
{
    // This function IS the credential provider
    return function () {
        // Use credentials from environment variables, if available
        $key = getenv(self::ENV_KEY);
        $secret = getenv(self::ENV_SECRET);
        if ($key && $secret) {
            return Promise\promise_for(
                new Credentials($key, $secret, getenv(self::ENV_SESSION))
            );
        }

        $msg = 'Could not find environment variable '
            . 'credentials in ' . self::ENV_KEY . '/' . self::ENV_SECRET;
        return new RejectedPromise(new CredentialsException($msg));
    };
}
```

## defaultProvider provider<a name="defaultprovider-provider"></a>

 `Aws\Credentials\CredentialProvider::defaultProvider` is the default credential provider\. This provider is used if you omit a `credentials` option when creating a client\. It first attempts to load credentials from environment variables, then from an \.ini file \(an `.aws/credentials` file first, followed by an `.aws/config` file\), and then from an instance profile \(`EcsCredentials` first, followed by `Ec2` metadata\)\.

**Note**  
The result of the default provider is automatically memoized\.

## ecsCredentials provider<a name="ecscredentials-provider"></a>

 `Aws\Credentials\CredentialProvider::ecsCredentials` attempts to load credentials by a `GET` request, whose URI is specified by the environment variable `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI` in the container\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

$provider = CredentialProvider::ecsCredentials();
// Be sure to memoize the credentials
$memoizedProvider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $memoizedProvider
]);
```

## env provider<a name="env-provider"></a>

 `Aws\Credentials\CredentialProvider::env` attempts to load credentials from environment variables\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => CredentialProvider::env()
]);
```

## assumeRoleWithWebIdentityCredentialProvider provider<a name="assume-role-with-web-identity-provider"></a>

 `Aws\Credentials\CredentialProvider::assumeRoleWithWebIdentityCredentialProvider` attempts to load credentials by assuming a role\. If the environment variables `AWS_ROLE_ARN` and `AWS_WEB_IDENTITY_TOKEN_FILE` are present, the provider will attempt to assume the role specified at `AWS_ROLE_ARN` using the token on disk at the full path specified in `AWS_WEB_IDENTITY_TOKEN_FILE`\. If environment variables are used, the provider will attempt to set the session from the `AWS_ROLE_SESSION_NAME` environment variable\.

If environment variables are not set, the provider will use the default profile, or the one set as `AWS_PROFILE`\. The provider reads profiles from `~/.aws/credentials` and `~/.aws/config` by default, and can read from profiles specified in the `filename` config option\. The provider will assume the role in `role_arn` of the profile, reading a token from the full path set in `web_identity_token_file`\. `role_session_name` will be used if set on the profile\.

The provider is called as part of the default chain and can be called directly\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

$provider = CredentialProvider::assumeRoleWithWebIdentityCredentialProvider();
// Cache the results in a memoize function to avoid loading and parsing
// the ini file on every API operation
$provider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

By default, this credential provider will inherit the configured region which will be used by the StsClient to assume the role\. Optionally, a full StsClient can be provided\. Credentials should be set as `false` on any provided StsClient\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;
use Aws\Sts\StsClient;

$stsClient = new StsClient([
    'region'      => 'us-west-2',
    'version'     => 'latest',
    'credentials' => false
])

$provider = CredentialProvider::assumeRoleWithWebIdentityCredentialProvider([
    'stsClient' => $stsClient
]);
// Cache the results in a memoize function to avoid loading and parsing
// the ini file on every API operation
$provider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

## ini provider<a name="ini-provider"></a>

 `Aws\Credentials\CredentialProvider::ini` attempts to load credentials from an [ini credential file](guide_credentials_profiles.md)\. By default, the SDK attempts to load the “default” profile from a file located at `~/.aws/credentials`\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

$provider = CredentialProvider::ini();
// Cache the results in a memoize function to avoid loading and parsing
// the ini file on every API operation
$provider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

You can use a custom profile or \.ini file location by providing arguments to the function that creates the provider\.

```
$profile = 'production';
$path = '/full/path/to/credentials.ini';

$provider = CredentialProvider::ini($profile, $path);
$provider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

## process provider<a name="process-provider"></a>

 `Aws\Credentials\CredentialProvider::process` attempts to load credentials from a credential\_process specified in an [ini credential file](guide_credentials_profiles.md)\. By default, the SDK attempts to load the “default” profile from a file located at `~/.aws/credentials`\. The SDK will call the credential\_process command exactly as given and then read JSON data from stdout\. The credential\_process must write credentials to stdout in the following format:

```
{
    "Version": 1,
    "AccessKeyId": "",
    "SecretAccessKey": "",
    "SessionToken": "",
    "Expiration": ""
}
```

 `SessionToken` and `Expiration` are optional\. If present, the credentials will be treated as temporary\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

$provider = CredentialProvider::process();
// Cache the results in a memoize function to avoid loading and parsing
// the ini file on every API operation
$provider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

You can use a custom profile or \.ini file location by providing arguments to the function that creates the provider\.

```
$profile = 'production';
$path = '/full/path/to/credentials.ini';

$provider = CredentialProvider::process($profile, $path);
$provider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $provider
]);
```

## instanceProfile provider<a name="instanceprofile-provider"></a>

 `Aws\Credentials\CredentialProvider::instanceProfile` attempts to load credentials from Amazon EC2 instance profiles\.

```
use Aws\Credentials\CredentialProvider;
use Aws\S3\S3Client;

$provider = CredentialProvider::instanceProfile();
// Be sure to memoize the credentials
$memoizedProvider = CredentialProvider::memoize($provider);

$client = new S3Client([
    'region'      => 'us-west-2',
    'version'     => '2006-03-01',
    'credentials' => $memoizedProvider
]);
```

By default, the provider retries fetching credentials up to three times\. The number of retries can be set with the `retries` option, and disabled entirely by setting the option to `0`\.

```
use Aws\Credentials\CredentialProvider;

$provider = CredentialProvider::instanceProfile([
    'retries' => 0
]);
$memoizedProvider = CredentialProvider::memoize($provider);
```

**Note**  
You can disable this attempt to load from Amazon EC2 instance profiles by setting the `AWS_EC2_METADATA_DISABLED` environment variable to `true`\.

## Memoizing credentials<a name="memoizing-credentials"></a>

At times you might need to create a credential provider that remembers the previous return value\. This can be useful for performance when loading credentials is an expensive operation or when using the `Aws\Sdk` class to share a credential provider across multiple clients\. You can add memoization to a credential provider by wrapping the credential provider function in a `memoize` function\.

```
use Aws\Credentials\CredentialProvider;

$provider = CredentialProvider::instanceProfile();
// Wrap the actual provider in a memoize function
$provider = CredentialProvider::memoize($provider);

// Pass the provider into the Sdk class and share the provider
// across multiple clients. Each time a new client is constructed,
// it will use the previously returned credentials as long as
// they haven't yet expired.
$sdk = new Aws\Sdk(['credentials' => $provider]);

$s3 = $sdk->getS3(['region' => 'us-west-2', 'version' => 'latest']);
$ec2 = $sdk->getEc2(['region' => 'us-west-2', 'version' => 'latest']);

assert($s3->getCredentials() === $ec2->getCredentials());
```

When the memoized credentials are expired, the memoize wrapper invokes the wrapped provider in an attempt to refresh the credentials\.