# Amazon S3 client\-side encryption with the AWS SDK for PHP Version 3<a name="s3-encryption-client"></a>

With client\-side encryption, data is encrypted and decrypted directly in your environment\. This means that this data is encrypted before it’s transferred to Amazon S3, and you don’t rely on an external service to handle encryption for you\. For new implementations, we suggest the use of `S3EncryptionClientV2` and `S3EncryptionMultipartUploaderV2` over the deprecated `S3EncryptionClient` and `S3EncryptionMultipartUploader`\. It is recommended that older implementations still using the deprecated versions attempt to migrate\. `S3EncryptionClientV2` maintains support for decrypting data that was encrypted using the legacy `S3EncryptionClient`\.

The AWS SDK for PHP implements [envelope encryption](https://docs.aws.amazon.com/kms/latest/developerguide/workflow.html) and uses [OpenSSL](https://www.openssl.org/) for its encrypting and decrypting\. The implementation is interoperable with [other SDKs that match its feature support](https://docs.aws.amazon.com/general/latest/gr/aws_sdk_cryptography.html)\. It’s also compatible with [the SDK’s promise\-based asynchronous workflow](guide_promises.md)\.

## Migration guide<a name="migration-guide"></a>

For those who are trying to migrate to from the deprecated clients to the new clients, there is a migration guide which can be found [here](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/s3-encryption-migration.html)\.

## Setup<a name="setup"></a>

To get started with client\-side encryption, you need the following:
+ An [AWS KMS encryption key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) 
+ An [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) 

Before running any example code, configure your AWS credentials\. See [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\.

## Encryption<a name="encryption"></a>

Uploading an encrypted object in `S3EncryptionClientV2` takes three additional parameters on top of the standard `PutObject` parameters:
+  `'@KmsEncryptionContext'` is a key\-value pair which can be used to add an extra layer of security to your encrypted object\. The encryption client must pass in the same key, which it will automatically do on a get call\. If no additional context is desired, pass in an empty array\.
+  `@CipherOptions` are additional configurations for the encryption including which cipher to use and keysize\.
+  `@MaterialsProvider` is a provider which handles generating a cipher key and initialization vector, as well as encrypting your cipher key\.

```
use Aws\S3\S3Client;
use Aws\S3\Crypto\S3EncryptionClientV2;
use Aws\Kms\KmsClient;
use Aws\Crypto\KmsMaterialsProviderV2;

 // Let's construct our S3EncryptionClient using an S3Client
 $encryptionClient = new S3EncryptionClientV2(
     new S3Client([
         'profile' => 'default',
         'region' => 'us-east-1',
         'version' => 'latest',
     ])
 );

 $kmsKeyId = 'kms-key-id';
 $materialsProvider = new KmsMaterialsProviderV2(
     new KmsClient([
         'profile' => 'default',
         'region' => 'us-east-1',
         'version' => 'latest',
     ]),
     $kmsKeyId
 );

 $bucket = 'the-bucket-name';
 $key = 'the-file-name';
 $cipherOptions = [
     'Cipher' => 'gcm',
     'KeySize' => 256,
     // Additional configuration options
 ];

 $result = $encryptionClient->putObject([
     '@MaterialsProvider' => $materialsProvider,
     '@CipherOptions' => $cipherOptions,
     '@KmsEncryptionContext' => ['context-key' => 'context-value'],
     'Bucket' => $bucket,
     'Key' => $key,
     'Body' => fopen('file-to-encrypt.txt', 'r'),
 ]);
```

**Note**  
In addition to the Amazon S3 and AWS KMS\-based service errors, you might receive thrown `InvalidArgumentException` objects if your `'@CipherOptions'` are not correctly configured\.

## Decryption<a name="decryption"></a>

Downloading and decrypting an object has four additional parameters, two of which are required, on top of the standard `GetObject` parameters\. The client will detect the basic cipher options for you\.
+   
** `'@SecurityProfile'`: If set to ‘V2’, only objects that are encrypted in V2\-compatible**  
format can be decrypted\. Setting this parameter to ‘V2\_AND\_LEGACY’ also allows objects encrypted in V1\-compatible format to be decrypted\. To support migration, set @SecurityProfile to ‘V2\_AND\_LEGACY’\. Use ‘V2’ only for new application development\.
+   
** `'@MaterialsProvider'` is a provider which handles generating a cipher key and initialization vector, as**  
well as encrypting your cipher key\.
+   
** `'@KmsAllowDecryptWithAnyCmk'`: \(optional\) Setting this parameter to true enables decryption**  
without supplying a KMS key id to the constructor of the MaterialsProvider\. The default value is false\.
+   
** `'@CipherOptions'` \(optional\) are additional configurations for the encryption including which**  
cipher to use and keysize\.

```
$result = $encryptionClient->getObject([
    '@KmsAllowDecryptWithAnyCmk' => true,
    '@SecurityProfile' => 'V2_AND_LEGACY',
    '@MaterialsProvider' => $materialsProvider,
    '@CipherOptions' => $cipherOptions,
    'Bucket' => $bucket,
    'Key' => $key,
]);
```

**Note**  
In addition to the Amazon S3 and AWS KMS\-based service errors, you might receive thrown `InvalidArgumentException` objects if your `'@CipherOptions'` are not correctly configured\.

## Cipher configuration<a name="cipher-configuration"></a>

** `'Cipher'` \(string\)**  
Cipher method that the encryption client uses while encrypting\. Only ‘gcm’ is supported at this time\.

**Important**  
PHP is [updated in version 7\.1](http://php.net/manual/en/migration71.new-features.php) to include the extra parameters necessary to [encrypt](http://php.net/manual/en/function.openssl-encrypt.php) and [decrypt](http://php.net/manual/en/function.openssl-decrypt.php) using OpenSSL for GCM encryption\. For PHP versions 7\.0 and earlier, a polyfill for GCM support is provided and used by the encryption clients `S3EncryptionClientV2` and `S3EncryptionMultipartUploaderV2`\. However, the performance for large inputs will be much slower using the polyfill than using the native implementation for PHP 7\.1\+, so upgrading older PHP version environments may be necessary to use them effectively\.

** `'KeySize'` \(int\)**  
The length of the content encryption key to generate for encrypting\. Defaults to 256 bits\. Valid configuration options are 256 and 128 bits\.

** `'Aad'` \(string\)**  
Optional ‘Additional authentication data’ to include with your encrypted payload\. This information is validated on decryption\. `Aad` is available only when using the ‘gcm’ cipher\.

**Important**  
Additional authentication data is not supported by all AWS SDKs and as such other SDKs may not be able to decrypt files encrypted using this parameter\.

## Metadata strategies<a name="metadata-strategies"></a>

You also have the option of providing an instance of a class that implements the `Aws\Crypto\MetadataStrategyInterface`\. This simple interface handles saving and loading the `Aws\Crypto\MetadataEnvelope` that contains your envelope encryption materials\. The SDK provides two classes that implement this: `Aws\S3\Crypto\HeadersMetadataStrategy` and `Aws\S3\Crypto\InstructionFileMetadataStrategy`\. `HeadersMetadataStrategy` is used by default\.

```
$strategy = new InstructionFileMetadataStrategy(
    $s3Client
);

$encryptionClient->putObject([
    '@MaterialsProvider' => $materialsProvider,
    '@MetadataStrategy' => $strategy,
    '@KmsEncryptionContext' => [],
    '@CipherOptions' => $cipherOptions,
    'Bucket' => $bucket,
    'Key' => $key,
    'Body' => fopen('file-to-encrypt.txt', 'r'),
]);

$result = $encryptionClient->getObject([
    '@KmsAllowDecryptWithAnyCmk' => false,
    '@MaterialsProvider' => $materialsProvider,
    '@SecurityProfile' => 'V2',
    '@MetadataStrategy' => $strategy,
    '@CipherOptions' => $cipherOptions,
    'Bucket' => $bucket,
    'Key' => $key,
]);
```

Class name constants for the `HeadersMetadataStrategy` and `InstructionFileMetadataStrategy` can also be supplied by invoking *::class*\.

```
$result = $encryptionClient->putObject([
    '@MaterialsProvider' => $materialsProvider,
    '@MetadataStrategy' => HeadersMetadataStrategy::class,
    '@CipherOptions' => $cipherOptions,
    'Bucket' => $bucket,
    'Key' => $key,
    'Body' => fopen('file-to-encrypt.txt', 'r'),
]);
```

**Note**  
If there is a failure after an instruction file is uploaded, it will not be automatically deleted\.

## Multipart uploads<a name="multipart-uploads"></a>

Performing a multipart upload with client\-side encryption is also possible\. The `Aws\S3\Crypto\S3EncryptionMultipartUploaderV2` prepares the source stream for encryption before uploading\. Creating one takes on a similar experience to using the `Aws\S3\MultipartUploader` and the `Aws\S3\Crypto\S3EncryptionClientV2`\. The `S3EncryptionMultipartUploaderV2` can handle the same `'@MetadataStrategy'` option as the `S3EncryptionClientV2`, as well as all available `'@CipherOptions'` configurations\.

```
$kmsKeyId = 'kms-key-id';
$materialsProvider = new KmsMaterialsProviderV2(
    new KmsClient([
        'region' => 'us-east-1',
        'version' => 'latest',
        'profile' => 'default',
    ]),
    $kmsKeyId
);

$bucket = 'the-bucket-name';
$key = 'the-upload-key';
$cipherOptions = [
    'Cipher' => 'gcm'
    'KeySize' => 256,
    // Additional configuration options
];

$multipartUploader = new S3EncryptionMultipartUploaderV2(
    new S3Client([
        'region' => 'us-east-1',
        'version' => 'latest',
        'profile' => 'default',
    ]),
    fopen('large-file-to-encrypt.txt', 'r'),
    [
        '@MaterialsProvider' => $materialsProvider,
        '@CipherOptions' => $cipherOptions,
        'bucket' => $bucket,
        'key' => $key,
    ]
);
$multipartUploader->upload();
```

**Note**  
In addition to the Amazon S3 and AWS KMS\-based service errors, you might receive thrown `InvalidArgumentException` objects if your `'@CipherOptions'` are not correctly configured\.