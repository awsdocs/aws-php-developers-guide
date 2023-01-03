# Using hard\-coded credentials<a name="guide_credentials_hardcoded"></a>

When testing new services or debugging issues, developers often want to include AWS credentials when constructing the client\. See the following for an example of how to authenticate to AWS, but do so with caution\. [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md) lists many recommended ways to add credentials to your project safely\.

**Warning**  
Hard\-coding your credentials can be dangerous because it’s easy to commit your credentials into an SCM repository accidentally\. Adding credentials directly in your production code can potentially expose your credentials to more people than you intend\. It can also make it difficult to rotate credentials in the future\.

If you decide to hard\-code credentials to an SDK client, provide an associative array of “key”, “secret”, and optional “token” key\-value pairs to the “credentials” option of a client constructor\.

```
// Hard-coded credentials
$s3Client = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'credentials' => [
        'key'    => 'my-access-key-id',
        'secret' => 'my-secret-access-key',
    ],
]);
```