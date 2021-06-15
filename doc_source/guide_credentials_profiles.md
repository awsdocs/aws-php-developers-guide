# Using the AWS Credentials File and Credential Profiles<a name="guide_credentials_profiles"></a>

A credentials file is a plaintext file that contains your access keys\. The file must:
+ Be on the same machine on which you’re running your application\.
+ Be named `credentials`\.
+ Be located in the `.aws/` folder in your home directory\.

The home directory can vary by operating system\. On Windows, you can refer to your home directory by using the environment variable `%UserProfile%`\. On Unix\-like systems, you can use the environment variable `$HOME` or `~` \(tilde\)\.

If you already use this file for other SDKs and tools \(like the AWS CLI\), you don’t need to change anything to use the files in this SDK\. If you use different credentials for different tools or applications, you can use **profiles** to configure multiple access keys in the same configuration file\.

We use this method in all our PHP code examples\.

Using an AWS credentials file offers the following benefits:
+ Your projects’ credentials are stored outside of your projects, so there is no chance of accidentally committing them into version control\.
+ You can define and name multiple sets of credentials in one place\.
+ You can easily reuse the same credentials among projects\.
+ Other AWS SDKs and tools support, this same credentials file\. This allows you to reuse your credentials with other tools\.

The format of the AWS credentials file should look something like the following\.

```
[default]
aws_access_key_id = YOUR_AWS_ACCESS_KEY_ID
aws_secret_access_key = YOUR_AWS_SECRET_ACCESS_KEY

[project1]
aws_access_key_id = ANOTHER_AWS_ACCESS_KEY_ID
aws_secret_access_key = ANOTHER_AWS_SECRET_ACCESS_KEY
```

Each section \(e\.g\., `[default]`, `[project1]`\), represents a separate credential profile\. You can reference profiles from an SDK configuration file, or when you are instantiating a client, by using the `profile` option\.

```
use Aws\DynamoDb\DynamoDbClient;

// Instantiate a client with the credentials from the project1 profile
$client = new DynamoDbClient([
    'profile' => 'project1',
    'region'  => 'us-west-2',
    'version' => 'latest'
]);
```

If no credentials or profiles were explicitly provided to the SDK and no credentials were defined in environment variables, but a credentials file is defined, the SDK uses the “default” profile\. You can change the default profile by specifying an alternate profile name in the `AWS_PROFILE` environment variable\.

## Assume Role with Profile<a name="assume-role-with-profile"></a>

You can configure the AWS SDK for PHP to use an IAM role by defining a profile for the role in `~/.aws/credentials`\.

Create a new profile with the `role_arn` for the role you will assume\. Also include the `source_profile` of a profile with credentials that have permissions to assume the IAM role\.

Profile in `~/.aws/credentials`:

```
[default]
aws_access_key_id = YOUR_AWS_ACCESS_KEY_ID
aws_secret_access_key = YOUR_AWS_SECRET_ACCESS_KEY

[project1]
role_arn = arn:aws:iam::123456789012:role/testing
source_profile = default
role_session_name = OPTIONAL_SESSION_NAME
```

By setting the `AWS_PROFILE` environment variable, or `profile` option when instantiating a client, the role specified in `project1` will be assumed, using the `default` profile as the source credentials\.

Roles can also be assumed for profiles defined in `~/.aws/config`\. Setting the environment variable `AWS_SDK_LOAD_NONDEFAULT_CONFIG` enables loading profiles for assuming a role from `~/.aws/config`\. When enabled, profiles from both `~/.aws/config` and `~/.aws/credentials` will be loaded\. Profiles from `~/.aws/credentials` are loaded last and will take precedence over a profile from `~/.aws/config` with the same name\. Profiles from either location can serve as the `source_profile` or the profile to be assumed\.

Profile in `~/.aws/config`:

```
[profile project1]
role_arn = arn:aws:iam::123456789012:role/testing
source_profile = default
role_session_name = OPTIONAL_SESSION_NAME
```

Profile in `~/.aws/credentials`:

```
[project2]
aws_access_key_id = YOUR_AWS_ACCESS_KEY_ID
aws_secret_access_key = YOUR_AWS_SECRET_ACCESS_KEY
```

Using the above files, `[project1]` will be assumed using `[project2]` as the source credentials\.