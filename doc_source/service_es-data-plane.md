# Signing an Amazon OpenSearch Service search request with AWS SDK for PHP Version 3<a name="service_es-data-plane"></a>

Amazon OpenSearch Service is a managed service that makes it easy to deploy, operate, and scale Amazon OpenSearch Service, a popular open\-source search, and analytics engine\. OpenSearch Service offers direct access to the Amazon OpenSearch Service API\. This means that developers can use the tools with which they’re familiar, as well as robust security options, such as using IAM users and roles for access control\. Many Amazon OpenSearch Service clients support request signing, but if you’re using a client that doesn’t, you can sign arbitrary PSR\-7 requests with the built\-in credential providers and signers of the AWS SDK for PHP\.

The following examples show how to:
+ Sign a request with the AWS signing protocol using [SignatureV4](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Signature.SignatureV4.html#_signRequest)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Signing an OpenSearch Service request<a name="signing-an-es-request"></a>

OpenSearch Service uses [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. This means that you need to sign requests against the service’s signing name \(`es`, in this case\) and the AWS Region of your OpenSearch Service domain\. A full list of Regions supported by OpenSearch Service can be found [ on the AWS Regions and Endpoints page](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the Amazon Web Services General Reference\. However, in this example, we sign requests against an OpenSearch Service domain in the `us-west-2` region\.

You need to provide credentials, which you can do either with the SDK’s default provider chain or with any form of credentials described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. You’ll also need a [PSR\-7 request](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Psr.Http.Message.RequestInterface.html) \(assumed in the code below to be named `$psr7Request`\)\.

```
// Pull credentials from the default provider chain
$provider = Aws\Credentials\CredentialProvider::defaultProvider();
$credentials = call_user_func($provider)->wait();

// Create a signer with the service's signing name and Region
$signer = new Aws\Signature\SignatureV4('es', 'us-west-2');

// Sign your request
$signedRequest = $signer->signRequest($psr7Request, $credentials);
```