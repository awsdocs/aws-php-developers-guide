# Creating Data Streams Using the Kinesis Data Streams API and the AWS SDK for PHP Version 3<a name="kinesis-example-data-stream"></a>

Amazon Kinesis Data Streams allows you to send real\-time data\. Create a data producer with Kinesis Data Streams that delivers data to the configured destination every time you add data\.

For more information, see [Creating and Managing Streams](https://docs.aws.amazon.com/kinesis/latest/dev/working-with-streams.html.html) in the Amazon Kinesis Developer Guide\.

The following examples show how to:
+ Create a data stream using [CreateAlias](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#createstream)\.
+ Get details about a single data stream using [DescribeStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#describestream)\.
+ List existing data streams using [ListStreams](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#liststreams)\.
+ Send data to an existing data stream using [PutRecord](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#putrecord)\.
+ Delete a data stream using [DeleteStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#deletestream)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using Amazon Kinesis Developer Guide, see the [Amazon Kinesis Data Streams Developer Guide](https://docs.aws.amazon.com/kinesis/latest/dev/)\.

## Create a Data Stream Using a Kinesis Data Stream<a name="create-a-data-stream-using-a-ak-data-stream"></a>

Establish a Kinesis data stream where you can send information to be processed by Kinesis using the following code example\. Learn more about [Creating and Updating Data Streams](https://docs.aws.amazon.com/kinesis/latest/dev/amazon-kinesis-streams.html) in the Amazon Kinesis Developer Guide\.

To create a Kinesis data stream, use the [CreateStream](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_CreateStream.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kinesis\KinesisClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$kinesisClient = new Aws\Kinesis\KinesisClient([
    'profile' => 'default',
    'version' => '2013-12-02',
    'region' => 'us-east-2'
]);

$shardCount = 2;
$name = "my_stream_name";


try {
    $result = $kinesisClient->createStream([
        'ShardCount' => $shardCount,
        'StreamName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve a Data Stream<a name="retrieve-a-data-stream"></a>

Get details about an existing data stream using the following code example\. By default, this returns information about the first 10 shards connected to the specified Kinesis data stream\. Remember to check `StreamStatus` from the response before writing data to a Kinesis data stream\.

To retrieve details about a specified Kinesis data stream, use the [DescribeStream](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_DescribeStream.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kinesis\KinesisClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$kinesisClient = new Aws\Kinesis\KinesisClient([
    'profile' => 'default',
    'version' => '2013-12-02',
    'region' => 'us-east-2'
]);

$name = "my_stream_name";

try {
    $result = $kinesisClient->describeStream([
        'StreamName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List Existing Data Streams That Are Connected to Kinesis<a name="list-existing-data-streams-that-are-connected-to-ak"></a>

List the first 10 data streams from your AWS account in the selected AWS Region\. Use the returned ``HasMoreStreams` to determine if there are more streams associated with your account\.

To list your Kinesis data streams, use the [ListStreams](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_ListStreams.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kinesis\KinesisClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$kinesisClient = new Aws\Kinesis\KinesisClient([
    'profile' => 'default',
    'version' => '2013-12-02',
    'region' => 'us-east-2'
]);

try {
    $result = $kinesisClient->listStreams([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Send Data to an Existing Data Stream<a name="send-data-to-an-existing-data-stream"></a>

Once you create a data stream, use the following example to send data\. Before sending data to it, use `DescribeStream` to check whether the data `StreamStatus` is active\.

To write a single data record to a Kinesis data stream, use the [PutRecord](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecord.html) operation\. To write up to 500 records into a Kinesis data stream, use the [PutRecords](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kinesis\KinesisClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$kinesisClient = new Aws\Kinesis\KinesisClient([
    'profile' => 'default',
    'version' => '2013-12-02',
    'region' => 'us-east-1'
]);

$name = "my_stream_name";
$content = '{"ticker_symbol":"QXZ", "sector":"HEALTHCARE", "change":-0.05, "price":84.51}';
$groupID = "input to a hash function that maps the partition key (and associated data) to a specific shard";

try {
    $result = $kinesisClient->PutRecord([
        'Data' => $content,
        'StreamName' => $name,
        'PartitionKey' => $groupID
    ]);
    print("<p>ShardID = " . $result["ShardId"] . "</p>");
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete a Data Stream<a name="delete-a-data-stream"></a>

This example demonstrates how to delete a data stream\. Deleting a data stream also deletes any data you sent to the data stream\. Active Kinesis data streams switch to the DELETING state until the stream deletion is complete\. While in the DELETING state, the stream continues to process data\.

To delete a Kinesis data stream, use the [DeleteStream](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_DeleteStream.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kinesis\KinesisClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$kinesisClient = new Aws\Kinesis\KinesisClient([
    'profile' => 'default',
    'version' => '2013-12-02',
    'region' => 'us-east-2'
]);

$name = "my_stream_name";

try {
    $result = $kinesisClient->deleteStream([
        'StreamName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```