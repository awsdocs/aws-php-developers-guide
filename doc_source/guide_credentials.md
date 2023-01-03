# Credentials for the AWS SDK for PHP Version 3<a name="guide_credentials"></a>

For reference information on available credentials mechanisms for the AWS SDKs, see [Credentials and access](https://docs.aws.amazon.com/sdkref/latest/guide/access.html) in the *AWS SDKs and Tools Reference Guide*\.

To make requests to Amazon Web Services, supply [AWS access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html), also known as credentials, to the AWS SDK for PHP\.

You can do this in the following ways:
+ Use the default credential provider chain *\(recommended\)*\.
+ Use a specific credential provider or provider chain \(or create your own\)\.
+ Supply the credentials yourself\. These can be root account credentials, IAM credentials, or temporary credentials retrieved from AWS STS\.

**Important**  
For security, we *strongly recommend* that you do **not** use the root account for AWS access\. Always refer to the [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide* for the latest security recommendations\.

## Using the default credential provider chain<a name="default-credential-chain"></a>

When you initialize a new service client without providing any credential arguments, the SDK uses the default credential provider chain to find AWS credentials\. The SDK uses the first provider in the chain that returns credentials without an error\.

The default provider chain looks for and uses credentials as follows, in this order:

1.  [Use credentials from environment variables](guide_credentials_environment.md)\.

   Setting environment variables is useful if you’re doing development work on a machine other than an Amazon EC2 instance\.

1.  [Use the AWS shared credentials file and profiles](guide_credentials_profiles.md)\.

   This credentials file is the same one used by other SDKs and the AWS CLI\. If you’re already using a shared credentials file, you can use that file for this purpose\.

   We use this method in most of our PHP code examples\.

1.  [Assume an IAM role](guide_credentials_assume_role.md)\.

   IAM roles provide applications on the instance with temporary security credentials to make AWS calls\. For example, IAM roles offer an easy way to distribute and manage credentials on multiple Amazon EC2 instances\.

## Other ways to add credentials<a name="other-credentials"></a>

You can also add credentials in these ways:
+  [Using a credential provider](guide_credentials_provider.md)\.

  Provide custom logic for credentials when constructing the client\.
+  [Using temporary credentials from AWS STS](guide_credentials_temporary.md)\.

  When using a multi\-factor authentication \(MFA\) token for two\-factor authentication, use AWS STS to give the user temporary credentials to access AWS services or use the AWS SDK for PHP\.
+  [Using hard\-coded credentials](guide_credentials_hardcoded.md) \(not recommended\)\.

**Warning**  
Hard\-coding your credentials can be dangerous, because it’s easy to accidentally commit your credentials into an SCM repository\. This can potentially expose your credentials to more people than you intend\. It can also make it difficult to rotate credentials in the future\. Do not submit code with hard\-coded credentials to your source control\.
+  [Creating anonymous clients](guide_credentials_anonymous.md)\.

  Create a client that isn’t associated with any credentials when the service allows anonymous access\.