# Getting Your Account\-Specific Endpoint for AWS Elemental MediaConvert with AWS SDK for PHP Version 3<a name="emc-examples-getendpoint"></a>

In this example, you use the AWS SDK for PHP Version 3 to call AWS Elemental MediaConvert and retrieve your account\-specific endpoint\. You can retrieve your endpoint URL from the service default endpoint and so do not yet need your acccount\-specific endpoint\.

The following examples show how to:
+ Retrieve your account\-specific endpoint\. using [DescribeEndpoints](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html#describeendpoints)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

To access the MediaConvert client, create an IAM role that gives AWS Elemental MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set Up IAM Permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the [AWS Elemental MediaConvert User Guide](https://docs.aws.amazon.com/mediaconvert/latest/ug/)\.

## Retrieve Endpoints<a name="retrieve-endpoints"></a>

Create an object to pass the empty request parameters for the describeEndpoints method of the `AWS.MediaConvert` client class\. To call the describeEndpoints method, create a promise for invoking an AWS Elemental MediaConvert service object, passing the parameters\. Handle the response in the promise callback\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\MediaConvert\MediaConvertClient;  
use Aws\Exception\AwsException;
```

 **Sample Code** 

Define the region in which to get the endpoint, and create an MediaConvert client object:

```
$client = new Aws\MediaConvert\MediaConvertClient([
    'profile' => 'default',
    'version' => '2017-08-29',
    'region' => 'us-east-2'
]);

//retrieve endpoint
try {
    $result = $client->describeEndpoints([]);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

Call the describeEndpoints method to retrieve the endpoints and save the endpoint’s URL:

```
$single_endpoint_url = $result['Endpoints'][0]['Url'];

print("Your endpoint is " . $single_endpoint_url);

//Create an AWSMediaConvert client object with the endpoint URL that you retrieved: 
$mediaConvertClient = new MediaConvertClient([
    'version' => '2017-08-29',
    'region' => 'us-east-2',
    'profile' => 'default',
    'endpoint' => $single_endpoint_url
]);
```