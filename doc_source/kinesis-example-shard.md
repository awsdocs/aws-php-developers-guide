# Manage data shards using the Kinesis Data Streams API and the AWS SDK for PHP Version 3<a name="kinesis-example-shard"></a>

Amazon Kinesis Data Streams enables you to send real\-time data to an endpoint\. The rate of data flow depends on the number of shards in your stream\.

You can write 1,000 records per second to a single shard\. Each shard also has an upload limit of 1 MiB per second\. Usage is calculated and charged on a per\-shard basis, so use these examples to manage the data capacity and cost of your stream\.

The following examples show how to:
+ List shards in a stream using [ListShards](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#listshards)\.
+ Add or reduce the number of shards in a stream using [UpdateShardCount](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kinesis-1913-12-02.html#updateshardcount)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

For more information about using Amazon Kinesis Data Streams, see the [Amazon Kinesis Data Streams Developer Guide](https://docs.aws.amazon.com/streams/latest/dev/)\.

## List data stream shards<a name="list-data-stream-shards"></a>

List the details of up to 100 shards in a specific stream\.

To list the shards in a Kinesis data stream, use the [ListShards](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_ListShards.html) operation\.

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
    $result = $kinesisClient->ListShards([
        'StreamName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Add more data stream shards<a name="add-more-data-stream-shards"></a>

If you need more data stream shards, you can increase your current number of shards\. We recommend that you double your shard count when increasing\. This makes a copy of each shard currently available to increase your capacity\. You can double the number of your shards only twice in one 24\-hour period\.

Remember that billing for Kinesis Data Streams usage is calculated per shard, so when demand decreases, we recommend that you reduce your shard count by half\. When you remove shards, you can only scale down the amount of shards to half of your current shard count\.

To update the shard count of a Kinesis data stream, use the [UpdateShardCount](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_UpdateShardCount.html) operation\.

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
$totalshards = 4;


try {
    $result = $kinesisClient->UpdateShardCount([
        'ScalingType' => 'UNIFORM_SCALING',
        'StreamName' => $name,
        'TargetShardCount' => $totalshards
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```