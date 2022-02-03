# Creating Delivery Streams Using the Kinesis Data Firehose API and the AWS SDK for PHP Version 3<a name="kinesis-firehose-example-delivery-stream"></a>

Amazon Kinesis Data Firehose enables you to send real\-time data to other AWS services including Amazon Kinesis Data Streams, Amazon S3, Amazon OpenSearch Service \(OpenSearch Service\), and Amazon Redshift, or to Splunk\. Create a data producer with delivery streams to deliver data to the configured destination every time you add data\.

The following examples show how to:
+ Create a delivery stream using [CreateDeliveryStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-firehose-2015-08-04.html#createdeliverystream)\.
+ Get details about a single delivery stream using [DescribeDeliveryStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-firehose-2015-08-04.html#describedeliverystream)\.
+ List your delivery streams using [ListDeliveryStreams](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-firehose-2015-08-04.html#listdeliverystreams)\.
+ Send data to a delivery stream using [PutRecord](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-firehose-2015-08-04.html#putrecord)\.
+ Delete a delivery stream using [DeleteDeliveryStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-firehose-2015-08-04.html#deletedeliverystream)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using Amazon Kinesis Data Firehose, see the [Amazon Kinesis Data Firehose Developer Guide](https://docs.aws.amazon.com/streams/latest/dev/)\.

## Create a Delivery Stream Using a Kinesis Data Stream<a name="create-a-delivery-stream-using-a-ak-data-stream"></a>

To establish a delivery stream that puts data into an existing Kinesis data stream, use the [CreateDeliveryStream](https://docs.aws.amazon.com/firehose/latest/APIReference/API_CreateDeliveryStream.html) operation\.

This enables developers to migrate existing Kinesis services to Kinesis Data Firehose\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);

$name = "my_stream_name";
$stream_type = "KinesisStreamAsSource";
$kinesis_stream = "arn:aws:kinesis:us-east-2:0123456789:stream/my_stream_name";
$role = "arn:aws:iam::0123456789:policy/Role";

try {
    $result = $firehoseClient->createDeliveryStream([
        'DeliveryStreamName' => $name,
        'DeliveryStreamType' => $stream_type,
        'KinesisStreamSourceConfiguration' => [
            'KinesisStreamARN' => $kinesis_stream,
            'RoleARN' => $role,
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Create a Delivery Stream Using an Amazon S3 Bucket<a name="create-a-delivery-stream-using-an-s3-bucket"></a>

To establish a delivery stream that puts data into an existing Amazon S3 bucket, use the [CreateDeliveryStream](https://docs.aws.amazon.com/firehose/latest/APIReference/API_CreateDeliveryStream.html) operation\.

Provide the destination parameters, as described in [Destination Parameters](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html)\. Then ensure that you grant Kinesis Data Firehose access to your Amazon S3 bucket, as described in [Grant Kinesis Data Firehose Access to an Amazon S3 Destination](https://docs.aws.amazon.com/firehose/latest/dev/controlling-access.html#using-iam-s3.html)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);

$name = "my_S3_stream_name";
$stream_type = "DirectPut";
$s3bucket = 'arn:aws:s3:::bucket_name';
$s3Role = 'arn:aws:iam::0123456789:policy/Role';

try {
    $result = $firehoseClient->createDeliveryStream([
        'DeliveryStreamName' => $name,
        'DeliveryStreamType' => $stream_type,
        'S3DestinationConfiguration' => [
            'BucketARN' => $s3bucket,
            'CloudWatchLoggingOptions' => [
                'Enabled' => false,
            ],
            'RoleARN' => $s3Role
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Create a Delivery Stream Using OpenSearch Service<a name="create-a-delivery-stream-using-es"></a>

To establish a Kinesis Data Firehose delivery stream that puts data into an OpenSearch Service cluster, use the [CreateDeliveryStream](https://docs.aws.amazon.com/firehose/latest/APIReference/API_CreateDeliveryStream.html) operation\.

Provide the destination parameters, as described in [Destination Parameters](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html)\. Ensure that you grant Kinesis Data Firehose access to your OpenSearch Service cluster, as described in [Grant Kinesis Data Firehose Access to an Amazon ES Destination](https://docs.aws.amazon.com/firehose/latest/dev/controlling-access.html#using-iam-es.html)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);

$name = "my_ES_stream_name";
$stream_type = "DirectPut";
$esDomainARN = 'arn:aws:es:us-east-2:0123456789:domain/Name';
$esRole = 'arn:aws:iam::0123456789:policy/Role';
$esIndex = 'root';
$esType = 'PHP_SDK';
$s3bucket = 'arn:aws:s3:::bucket_name';
$s3Role = 'arn:aws:iam::0123456789:policy/Role';

try {
    $result = $firehoseClient->createDeliveryStream([
        'DeliveryStreamName' => $name,
        'DeliveryStreamType' => $stream_type,
        'ElasticsearchDestinationConfiguration' => [
            'DomainARN' => $esDomainARN,
            'IndexName' => $esIndex,
            'RoleARN' => $esRole,
            'S3Configuration' => [

                'BucketARN' => $s3bucket,
                'CloudWatchLoggingOptions' => [
                    'Enabled' => false,
                ],
                'RoleARN' => $s3Role,

            ],

            'TypeName' => $esType,
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve a Delivery Stream<a name="retrieve-a-delivery-stream"></a>

To get the details about an existing Kinesis Data Firehose delivery stream, use the [DescribeDeliveryStream](https://docs.aws.amazon.com/firehose/latest/APIReference/API_DescribeDeliveryStream.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);

$name = "my_stream_name";

try {
    $result = $firehoseClient->describeDeliveryStream([
        'DeliveryStreamName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List Existing Delivery Streams Connected to Kinesis Data Streams<a name="list-existing-delivery-streams-connected-to-aks"></a>

To list all the existing Kinesis Data Firehose delivery streams sending data to Kinesis Data Streams, use the [ListDeliveryStreams](https://docs.aws.amazon.com/firehose/latest/APIReference/API_ListDeliveryStreams.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);


try {
    $result = $firehoseClient->listDeliveryStreams([
        'DeliveryStreamType' => 'KinesisStreamAsSource',
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List Existing Delivery Streams Sending Data to Other AWS Services<a name="list-existing-delivery-streams-sending-data-to-other-aws-services"></a>

To list all the existing Kinesis Data Firehose delivery streams sending data to Amazon S3, OpenSearch Service, or Amazon Redshift, or to Splunk, use the [ListDeliveryStreams](https://docs.aws.amazon.com/firehose/latest/APIReference/API_ListDeliveryStreams.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);


try {
    $result = $firehoseClient->listDeliveryStreams([
        'DeliveryStreamType' => 'DirectPut',
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Send Data to an Existing Kinesis Data Firehose Delivery Stream<a name="send-data-to-an-existing-akf-delivery-stream"></a>

To send data through a Kinesis Data Firehose delivery stream to your specified destination, use the [PutRecord](https://docs.aws.amazon.com/firehose/latest/APIReference/API_API_PutRecord.html) operation after you create a Kinesis Data Firehose delivery stream\.

Before sending data to a Kinesis Data Firehose delivery stream, use `DescribeDeliveryStream` to see if the delivery stream is active\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);

$name = "my_stream_name";
$content = '{"ticker_symbol":"QXZ", "sector":"HEALTHCARE", "change":-0.05, "price":84.51}';

try {
    $result = $firehoseClient->putRecord([
        'DeliveryStreamName' => $name,
        'Record' => [
            'Data' => $content,
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete a Kinesis Data Firehose Delivery Stream<a name="delete-a-akf-delivery-stream"></a>

To delete a Kinesis Data Firehose delivery stream, use the [DeleteDeliveryStreams](https://docs.aws.amazon.com/firehose/latest/APIReference/API_DeleteDeliveryStreams.html) operation\. This also deletes any data you have sent to the delivery stream\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Firehose\FirehoseClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$firehoseClient = new Aws\Firehose\FirehoseClient([
    'profile' => 'default',
    'version' => '2015-08-04',
    'region' => 'us-east-2'
]);

$name = "my_stream_name";

try {
    $result = $firehoseClient->deleteDeliveryStream([
        'DeliveryStreamName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```