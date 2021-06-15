# Encrypting and Decrypting AWS KMS Data Keys Using the AWS SDK for PHP Version 3<a name="kms-example-encrypt"></a>

Data keys are encryption keys that you can use to encrypt data, including large amounts of data and other data encryption keys\.

You can use a AWS Key Management Service \(AWS KMS\) [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys.html) to generate, encrypt, and decrypt data keys\. However, AWS KMS does not store, manage, or track your data keys, or perform cryptographic operations with data keys\. Use and manage data keys outside of AWS KMS\.

The following examples show how to:
+ Encrypt a data key using [Encrypt](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#encrypt)\.
+ Decrypt a data key using [Decrypt](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#decrypt)\.
+ Re\-encrypt a data key with a new CMK using [ReEncrypt](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#reencrypt)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using AWS Key Management Service \(AWS KMS\), see the [AWS KMS Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

## Encrypt<a name="encrypt"></a>

The [Encrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html) operation is designed to encrypt data keys, but it’s not frequently used\. The [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html) and [GenerateDataKeyWithoutPlaintext](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKeyWithoutPlaintext.html) operations return encrypted data keys\. You might use the `Encypt` method when you’re moving encrypted data to a new AWS Region and want to encrypt its data key by using a CMK in the new Region\.

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
$message = pack('c*', 1, 2, 3, 4, 5, 6, 7, 8, 9, 0);

try {
    $result = $KmsClient->encrypt([
        'KeyId' => $keyId,
        'Plaintext' => $message,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Decrypt<a name="decrypt"></a>

To decrypt a data key, use the [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) operation\.

The `ciphertextBlob` that you specify must be the value of the `CiphertextBlob` field from a [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html), [GenerateDataKeyWithoutPlaintext](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKeyWithoutPlaintext.html), or [Encrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html) response\.

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

$ciphertext = 'Place your cipher text blob here';

try {
    $result = $KmsClient->decrypt([
        'CiphertextBlob' => $ciphertext,
    ]);
    $plaintext = $result['Plaintext'];
    var_dump($plaintext);
} catch (AwsException $e) {
    // Output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Reencrypt<a name="reencrypt"></a>

To decrypt an encrypted data key, and then immediately reencrypt the data key under a different CMK, use the [ReEncrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_ReEncrypt.html) operation\. The operations are performed entirely on the server side within AWS KMS, so they never expose your plaintext outside of AWS KMS\.

The `ciphertextBlob` that you specify must be the value of the `CiphertextBlob` field from a [GenerateDataKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html), [GenerateDataKeyWithoutPlaintext](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKeyWithoutPlaintext.html), or [Encrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html) response\.

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
$ciphertextBlob = 'Place your cipher text blob here';

try {
    $result = $KmsClient->reEncrypt([
        'CiphertextBlob' => $ciphertextBlob,
        'DestinationKeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```