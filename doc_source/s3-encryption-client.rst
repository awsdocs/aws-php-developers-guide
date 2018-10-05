.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

########################################################
|S3| Client-Side Encryption with the |sdk-php| Version 3
########################################################

.. meta::
   :description: Client-side encryption for the Amazon S3 client with the AWS SDK for PHP version 3.
   :keywords: AWS SDK for PHP version 3 constructor, AWS SDK for PHP version 3 client configuration

The |sdk-php| provides an ``S3EncryptionClient``. With client-side
encryption, data is encrypted and decrypted directly in your environment. This
means that this data is encrypted before it's transferred to |S3|, and you
don’t rely on an external service to handle encryption for you.

The |sdk-php| implements :KMS-dg:`envelope encryption <workflow>`
and uses `OpenSSL <https://www.openssl.org/>`_ for its encrypting and
decrypting. The implementation is interoperable with :AWS-gr:`other SDKs that match its feature support <aws_sdk_cryptography>`.
It's also compatible with :doc:`the SDK’s promise-based asynchronous workflow <guide_promises>`.

Setup
=====

To get started with client-side encryption, you need the following:

* An :KMS-dg:`AWS KMS encryption key <create-keys>`
* An :S3-gsg:`S3 bucket <CreatingABucket>`

Before running any example code, configure your AWS credentials. See :doc:`guide_credentials`.

Encryption
==========

Uploading an encrypted object through the ``PutObject`` operation takes a similar
interface and requires two new parameters.

.. code-block:: php

   use Aws\S3\S3Client;
   use Aws\S3\Crypto\S3EncryptionClient;
   use Aws\Kms\KmsClient;
   use Aws\Crypto\KmsMaterialsProvider;

    // Let's construct our S3EncryptionClient using an S3Client
    $encryptionClient = new S3EncryptionClient(
        new S3Client([
            'profile' => 'default',
            'region' => 'us-east-1',
            'version' => 'latest',
        ])
    );

    $kmsKeyArn = 'arn-to-the-kms-key';
    // This materials provider handles generating a cipher key and
    // initialization vector, as well as encrypting your cipher key via AWS KMS
    $materialsProvider = new KmsMaterialsProvider(
        new KmsClient([
            'profile' => 'default',
            'region' => 'us-east-1',
            'version' => 'latest',
        ]),
        $kmsKeyArn
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
        'Bucket' => $bucket,
        'Key' => $key,
        'Body' => fopen('file-to-encrypt.txt', 'r'),
    ]);

.. note::

    In addition to the |S3| and |KMS|-based service errors, you might
    receive thrown ``InvalidArgumentException`` objects if your
    ``'@CipherOptions'`` are not correctly configured.

Decryption
==========

Downloading and decrypting an object requires only one additional parameter on
top of ``GetObject``, and the client will detect the basic cipher options for you.
Additional configuration options are passed through for decryption.

.. code-block:: php

    $result = $encryptionClient->getObject([
        '@MaterialsProvider' => $materialsProvider,
        '@CipherOptions' => [
            // Additional configuration options
        ],
        'Bucket' => $bucket,
        'Key' => $key,
    ]);

.. note::

    In addition to the |S3| and |KMS|-based service errors, you might
    receive thrown ``InvalidArgumentException`` objects if your
    ``'@CipherOptions'`` are not correctly configured.

Cipher Configuration
====================

``'Cipher'`` (string)
    Cipher method that the encryption client uses while
    encrypting. Only 'gcm' and 'cbc' are supported at this time.

.. important::

    PHP is `updated in version 7.1 <http://php.net/manual/en/migration71.new-features.php>`_
    to include the extra parameters necessary to `encrypt <http://php.net/manual/en/function.openssl-encrypt.php>`_
    and `decrypt <http://php.net/manual/en/function.openssl-decrypt.php>`_
    using OpenSSL for GCM encryption. As a result, using GCM with your
    ``Aws\S3\Crypto\S3EncryptionClient`` is only available on PHP 7.1 or later.

``'KeySize'`` (int)
    The length of the content encryption key to generate for
    encrypting. Defaults to 256 bits. Valid configuration options are 256,
    192, and 128.

``'Aad'`` (string)
    Optional 'Additional authentication data' to include with your
    encrypted payload. This information is validated on decryption. ``Aad`` is
    available only when using the 'gcm' cipher.

Metadata Strategies
===================

You also have the option of providing an instance of a class that implements
the ``Aws\Crypto\MetadataStrategyInterface``. This simple interface handles
saving and loading the ``Aws\Crypto\MetadataEnvelope`` that contains your
envelope encryption materials. The SDK provides two classes that implement
this: ``Aws\S3\Crypto\HeadersMetadataStrategy`` and
``Aws\S3\Crypto\InstructionFileMetadataStrategy``. ``HeadersMetadataStrategy``
is used by default.

.. code-block:: php

    $strategy = new InstructionFileMetadataStrategy(
        $s3Client,
        '.instr'
    );

    $result = $encryptionClient->putObject([
        '@MaterialsProvider' => $materialsProvider,
        '@MetadataStrategy' => $strategy,
        '@CipherOptions' => $cipherOptions,
        'Bucket' => $bucket,
        'Key' => $key,
        'Body' => fopen('file-to-encrypt.txt'),
    ]);

Class name constants for the ``HeadersMetadataStrategy`` and
``InstructionFileMetadataStrategy`` can also be supplied by invoking
`::class`.

.. code-block:: php

    $result = $encryptionClient->putObject([
        '@MaterialsProvider' => $materialsProvider,
        '@MetadataStrategy' => HeadersMetadataStrategy::class,
        '@CipherOptions' => $cipherOptions,
        'Bucket' => $bucket,
        'Key' => $key,
        'Body' => fopen('file-to-encrypt.txt'),
    ]);

.. note::

    If there is a failure after an instruction file is uploaded, it will
    not be automatically deleted.

Multipart Uploads
=================

Performing a multipart upload with client-side encryption is also possible. The
``Aws\S3\Crypto\S3EncryptionMultipartUploader`` prepares the source stream for
for encryption before uploading. Creating one takes on a similar experience to
using the ``Aws\S3\MultipartUploader`` and the ``Aws\S3\Crypto\S3EncryptionClient``.
The ``S3EncryptionMultipartUploader`` can handle the same ``'@MetadataStrategy'``
option as the ``S3EncryptionClient``, as well as all available ``'@CipherOptions'``
configurations.

.. code-block:: php

    $kmsKeyArn = 'arn-to-the-kms-key';
    // This materials provider handles generating a cipher key and
    // initialization vector, as well as encrypting your cipher key via AWS KMS
    $materialsProvider = new KmsMaterialsProvider(
        new KmsClient([
            'region' => 'us-east-1',
            'version' => 'latest',
            'profile' => 'default',
        ]),
        $kmsKeyArn
    );

    $bucket = 'the-bucket-name';
    $key = 'the-upload-key';
    $cipherOptions = [
        'Cipher' => 'gcm'
        'KeySize' => 256,
        // Additional configuration options
    ];

    $multipartUploader = new S3EncryptionMultipartUploader(
        new S3Client([
            'region' => 'us-east-1',
            'version' => 'latest',
            'profile' => 'default',
        ]),
        fopen('large-file-to-encrypt.txt'),
        [
            '@MaterialsProvider' => $materialsProvider,
            '@CipherOptions' => $cipherOptions,
            'bucket' => 'bucket',
            'key' => 'key',
        ]
    );
    $multipartUploader->upload();

.. note::

    In addition to the |S3| and |KMS|-based service errors, you might
    receive thrown ``InvalidArgumentException`` objects if your
    ``'@CipherOptions'`` are not correctly configured.
