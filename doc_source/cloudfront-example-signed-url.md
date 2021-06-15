# Signing Amazon CloudFront URLs with AWS SDK for PHP Version 3<a name="cloudfront-example-signed-url"></a>

Signed URLs enable you to provide users access to your private content\. A signed URL includes additional information \(for example, expiration time\) that gives you more control over access to your content\. This additional information appears in a policy statement, which is based on either a canned policy or a custom policy\. For information about how to set up private distributions and why you need to sign URLs, see [Serving Private Content through Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/PrivateContent.html) in the Amazon CloudFront Developer Guide\.
+ Create a signed Amazon CloudFront URL using [getSignedURL](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.CloudFront.CloudFrontClient.html#_getSignedUrl)\.
+ Create a signed Amazon CloudFront cookie using [getSignedCookie](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.CloudFront.CloudFrontClient.html#_getSignedCookie)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using Amazon CloudFront, see the [Amazon CloudFront Developer Guide](Amazon CloudFront Developer Guide)\.

## Signing CloudFront URLs for Private Distributions<a name="signing-cf-urls-for-private-distributions"></a>

You can sign a URL using the CloudFront client in the SDK\. First, you must create a `CloudFrontClient` object\. You can sign a CloudFront URL for a video resource using either a canned or custom policy\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function signPrivateDistribution($cloudFrontClient, $resourceKey, $expires, 
    $privateKey, $keyPairId)
{
    try {
        $result = $cloudFrontClient->getSignedUrl([
            'url' => $resourceKey,
            'expires' => $expires,
            'private_key' => $privateKey,
            'key_pair_id' => $keyPairId
        ]);

        return $result;
    
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function signAPrivateDistribution()
{
    $resourceKey = 'https://d13l49jEXAMPLE.cloudfront.net/my-file.txt';
    $expires = time() + 300; // 5 minutes (5 * 60 seconds) from now.
    $privateKey = dirname(__DIR__) . '/cloudfront/my-private-key.pem';
    $keyPairId = 'AAPKAJIKZATYYYEXAMPLE';
    
    $cloudFrontClient = new CloudFrontClient([
        'profile' => 'default',
        'version' => '2014-11-06',
        'region' => 'us-east-1'
    ]);
    
    echo signPrivateDistribution($cloudFrontClient, $resourceKey, $expires, 
        $privateKey, $keyPairId);
}

// Uncomment the following line to run this code in an AWS account.
// signAPrivateDistribution();
```

## Use a Custom Policy When Creating CloudFront URLs<a name="use-a-custom-policy-when-creating-cf-urls"></a>

To use a custom policy, provide the `policy` key instead of `expires`\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function signPrivateDistributionPolicy($cloudFrontClient, $resourceKey, 
    $customPolicy, $privateKey, $keyPairId)
{
    try {
        $result = $cloudFrontClient->getSignedUrl([
            'url' => $resourceKey,
            'policy' => $customPolicy,
            'private_key' => $privateKey,
            'key_pair_id' => $keyPairId
        ]);

        return $result;

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function signAPrivateDistributionPolicy()
{
    $resourceKey = 'https://d13l49jEXAMPLE.cloudfront.net/my-file.txt';
    $expires = time() + 300; // 5 minutes (5 * 60 seconds) from now.
    $customPolicy = <<<POLICY
{
    "Statement": [
        {
            "Resource": "{$resourceKey}",
            "Condition": {
                "IpAddress": {"AWS:SourceIp": "{$_SERVER['REMOTE_ADDR']}/32"},
                "DateLessThan": {"AWS:EpochTime": {$expires}}
            }
        }
    ]
}
POLICY;
    $privateKey = dirname(__DIR__) . '/cloudfront/my-private-key.pem';
    $keyPairId = 'AAPKAJIKZATYYYEXAMPLE';
    
    $cloudFrontClient = new CloudFrontClient([
        'profile' => 'default',
        'version' => '2014-11-06',
        'region' => 'us-east-1'
    ]);
    
    echo signPrivateDistributionPolicy($cloudFrontClient, $resourceKey, 
        $customPolicy, $privateKey, $keyPairId);
}

// Uncomment the following line to run this code in an AWS account.
// signAPrivateDistributionPolicy();
```

## Use a CloudFront Signed URL<a name="use-a-cf-signed-url"></a>

The form of the signed URL differs, depending on whether the URL you are signing is using the “HTTP” or “RTMP” scheme\. In the case of “HTTP”, the full, absolute URL is returned\. For “RTMP”, only the relative URL is returned for your convenience\. This is because some players require the host and path to be provided as separate parameters\.

The following example shows how you could use the signed URL to construct a webpage that displays a video using [JWPlayer](http://www.longtailvideo.com/jw-player/)\. The same type of technique would apply to other players such as [FlowPlayer](http://flowplayer.org/), but require different client\-side code\.

```
<html>
<head>
    <title>|CFlong| Streaming Example</title>
    <script type="text/javascript" src="https://example.com/jwplayer.js"></script>
</head>
<body>
    <div id="video">The canned policy video will be here.</div>
    <script type="text/javascript">
        jwplayer('video').setup({
            file: "<?= $streamHostUrl ?>/cfx/st/<?= $signedUrlCannedPolicy ?>",
            width: "720",
            height: "480"
        });
    </script>
</body>
</html>
```

## Signing CloudFront Cookies for Private Distributions<a name="signing-cf-cookies-for-private-distributions"></a>

As an alternative to signed URLs, you can also grant clients access to a private distribution via signed cookies\. Signed cookies enable you to provide access to multiple restricted files, such as all of the files for a video in HLS format or all of the files in the subscribers’ area of a website\. For more information on why you might want to use signed cookies instead of signed URLs \(or vice versa\), see [Choosing Between Signed URLs and Signed Cookies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-choosing-signed-urls-cookies.html) in the Amazon CloudFront Developer Guide\.

Creating a signed cookie is similar to creating a signed URL\. The only difference is the method that is called \(`getSignedCookie` instead of `getSignedUrl`\)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function signCookie($cloudFrontClient, $resourceKey, $expires, 
    $privateKey, $keyPairId)
{
    try {
        $result = $cloudFrontClient->getSignedCookie([
            'url' => $resourceKey,
            'expires' => $expires, 
            'private_key' => $privateKey,
            'key_pair_id' => $keyPairId
        ]);

        return $result;

    } catch (AwsException $e) {
        return [ 'Error' => $e->getAwsErrorMessage() ];
    }
}

function signACookie()
{
    $resourceKey = 'https://d13l49jEXAMPLE.cloudfront.net/my-file.txt';
    $expires = time() + 300; // 5 minutes (5 * 60 seconds) from now.
    $privateKey = dirname(__DIR__) . '/cloudfront/my-private-key.pem';
    $keyPairId = 'AAPKAJIKZATYYYEXAMPLE';

    $cloudFrontClient = new CloudFrontClient([
        'profile' => 'default',
        'version' => '2014-11-06',
        'region' => 'us-east-1'
    ]);

    $result = signCookie($cloudFrontClient, $resourceKey, $expires, 
        $privateKey, $keyPairId);

    /* If successful, returns something like:
    CloudFront-Expires = 1589926678
    CloudFront-Signature = Lv1DyC2q...2HPXaQ__
    CloudFront-Key-Pair-Id = AAPKAJIKZATYYYEXAMPLE
    */
    foreach($result as $key => $value)
    {
        echo $key . ' = ' . $value . "\n";
    }
}

// Uncomment the following line to run this code in an AWS account.
// signACookie();
```

## Use a Custom Policy When Creating CloudFront Cookies<a name="use-a-custom-policy-when-creating-cf-cookies"></a>

As with `getSignedUrl`, you can provide a `'policy'` parameter instead of an `expires` parameter and a `url` parameter to sign a cookie with a custom policy\. A custom policy can contain wildcards in the resource key\. This enables you to create a single signed cookie for multiple files\.

 `getSignedCookie` returns an array of key\-value pairs, all of which must be set as cookies to grant access to a private distribution\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function signCookiePolicy($cloudFrontClient, $customPolicy, 
    $privateKey, $keyPairId)
{
    try {
        $result = $cloudFrontClient->getSignedCookie([
            'policy' => $customPolicy,
            'private_key' => $privateKey,
            'key_pair_id' => $keyPairId
        ]);
    
        return $result;

    } catch (AwsException $e) {
        return [ 'Error' => $e->getAwsErrorMessage() ];
    }
}

function signACookiePolicy()
{
    $resourceKey = 'https://d13l49jEXAMPLE.cloudfront.net/my-file.txt';
    $expires = time() + 300; // 5 minutes (5 * 60 seconds) from now.
    $customPolicy = <<<POLICY
{
    "Statement": [
        {
            "Resource": "{$resourceKey}",
            "Condition": {
                "IpAddress": {"AWS:SourceIp": "{$_SERVER['REMOTE_ADDR']}/32"},
                "DateLessThan": {"AWS:EpochTime": {$expires}}
            }
        }
    ]
}
POLICY;
    $privateKey = dirname(__DIR__) . '/cloudfront/my-private-key.pem';
    $keyPairId = 'AAPKAJIKZATYYYEXAMPLE';

    $cloudFrontClient = new CloudFrontClient([
        'profile' => 'default',
        'version' => '2014-11-06',
        'region' => 'us-east-1'
    ]);

    $result = signCookiePolicy($cloudFrontClient, $customPolicy, 
        $privateKey, $keyPairId); 

    /* If successful, returns something like:
    CloudFront-Policy = eyJTdGF0...fX19XX0_
    CloudFront-Signature = RowqEQWZ...N8vetw__
    CloudFront-Key-Pair-Id = AAPKAJIKZATYYYEXAMPLE
    */
    foreach ($result as $key => $value) {
        echo $key . ' = ' . $value . "\n";
    }
}

// Uncomment the following line to run this code in an AWS account.
// signACookiePolicy();
```

## Send CloudFront Cookies to Guzzle Client<a name="send-cf-cookies-to-guzzle-client"></a>

You can also pass these cookies to a `GuzzleHttp\Cookie\CookieJar` for use with a Guzzle client\.

```
use GuzzleHttp\Client;
use GuzzleHttp\Cookie\CookieJar;

$distribution = "example-distribution.cloudfront.net";
$client = new \GuzzleHttp\Client([
    'base_uri' => "https://$distribution",
    'cookies' => CookieJar::fromArray($signedCookieCustomPolicy, $distribution),
]);

$client->get('video.mp4');
```

For more information, see [Using Signed Cookies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-signed-cookies.html) in the Amazon CloudFront Developer Guide\.