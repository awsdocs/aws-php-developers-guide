# Working with Keys Using the AWS KMS API and the AWS SDK for PHP Version 3<a name="kms-example-keys"></a>

The primary resources in AWS Key Management Service \(AWS KMS\) are [customer master keys \(CMKs\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys.html)\. You can use a CMK to encrypt your data\.

The following examples show how to:
+ Create a customer CMK using [CreateKey](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#createkey)\.
+ Generate a data key using [GenerateDataKey](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#generatedatakey)\.
+ View a CMK using [DescribeKey](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#describekey)\.
+ Get key IDs and key ARNS of CMKs using [ListKeys](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#listkeys)\.
+ Enable CMKs using [EnableKey](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#enablekey)\.
+ Disable CMKs using [DisableKey](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#disablekey)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

For more information about using AWS Key Management Service \(AWS KMS\), see the [AWS KMS Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

## Create a CMK<a name="create-a-cmk"></a>

To create a [CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys.html), use the [CreateKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateKey.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

//Creates a customer master key (CMK) in the caller's AWS account.
$desc = "Key for protecting critical data";

try {
    $result = $KmsClient->createKey([
        'Description' => $desc,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Generate a Data Key<a name="generate-a-data-key"></a>

To generate a data encryption key, use the [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html) operation\. This operation returns plaintext and encrypted copies of the data key that it creates\. Specify the customer master key \(CMK\) under which to generate the data key\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
$keySpec = 'AES_256';

try {
    $result = $KmsClient->generateDataKey([
        'KeyId' => $keyId,
        'KeySpec' => $keySpec,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## View a CMK<a name="view-a-cmk"></a>

To get detailed information about a CMK, including the CMK’s Amazon Resource Name \(ARN\) and [key state](https://docs.aws.amazon.com/kms/latest/developerguide/key-state.html), use the [DescribeKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html) operation\.

 `DescribeKey` doesn’t get aliases\. To get aliases, use the [ListAliases](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListKeys.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';

try {
    $result = $KmsClient->describeKey([
        'KeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Get the Key ID and Key ARNs of a CMK<a name="get-the-key-id-and-key-arns-of-a-cmk"></a>

To get the ID and ARN of the CMK, use the [ListAliases](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListKeys.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$limit = 10;

try {
    $result = $KmsClient->listKeys([
        'Limit' => $limit,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Enable a CMK<a name="enable-a-cmk"></a>

To enable a disabled CMK, use the [EnableKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_EnableKey.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);


$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';

try {
    $result = $KmsClient->enableKey([
        'KeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Disable a CMK<a name="disable-a-cmk"></a>

To disable a CMK, use the [DisableKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DisableKey.html) operation\. Disabling a CMK prevents it from being used\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';

try {
    $result = $KmsClient->disableKey([
        'KeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```