.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#####################################
Amazon S3 Encryption Client Migration
#####################################

.. meta::
   :description: Describes how to migrate to the latest S3 encryption clients for |sdk-php|.

This topic shows how to migrate your applications from Version 1 (V1) of the Amazon Simple Storage Service (Amazon S3) encryption 
client to Version 2 (V2), and ensure application availability throughout the migration process.

Migration Overview
==================

This migration happens in two phases: 

1. **Update existing clients to read new formats.** First, deploy an updated version of the |sdk-php| 
to your application. This allows existing V1 encryption clients to decrypt objects written by the 
new V2 clients. If your application uses multiple AWS SDKs, you must upgrade each SDK separately. 

2. **Migrate encryption and decryption clients to V2.** Once all of your V1 encryption clients can 
read new formats, you can migrate your existing encryption and decryption clients to their respective 
V2 versions.


Update Existing Clients to Read New Formats
===========================================

The V2 encryption client uses encryption algorithms that older versions of the client don't support. 
The first step in the migration is to update your V1 decryption clients to the latest SDK release. 
After completing this step, your application’s V1 clients will be able to decrypt objects encrypted 
by V2 encryption clients.  See details below for each major version of the |sdk-php|.

Upgrading AWS SDK for PHP Version 3
------------------------------------

Version 3 is the latest version of the |sdk-php|. To complete this migration, you must 
use version 3.148.0 or later of the ``aws/aws-sdk-php`` package.

**Installing from the Command Line**

For projects that were installed using Composer, in the Composer file, update the SDK package to 
version 3.148.0 of the SDK and then run the following command.

.. code-block:: php

 composer update aws/aws-sdk-php

**Installing Using the Phar or Zip File**

Use one of the following methods. Be sure to place the updated SDK file in the location required by 
your code, which is determined by the require statement.

For projects that were installed using the Phar file, get the updated file from `https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar <https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar>`__.

.. code-block:: php

 <?php
   require '/path/to/aws.phar';
 ?>

For projects that were installed using the Zip file, get the updated file from `https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip <https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip>`__.

.. code-block:: php

 <?php
   require '/path/to/aws-autoloader.php';
 ?>

Migrate Encryption and Decryption Clients to V2
==============================================

After updating your clients to read the new encryption formats, you can update your applications to the V2 
encryption and decryption clients. The following steps show you how to successfully migrate your code from V1 to V2.

Requirements for Updating to V2 Clients
---------------------------------------

1. The AWS KMS encryption context must be passed into the ``S3EncryptionClientV2::putObject`` and ``S3EncryptionClientV2::putObjectAsync`` methods.
AWS KMS encryption context is an associative array of key-value  pairs, which you must add to the 
encryption context for AWS KMS key encryption. If no additional context is required, you can pass an empty array.

2. ``@SecurityProfile`` must be passed into the ``getObject``  and ``getObjectAsync`` methods in ``S3EncryptionClientV2``.
``@SecurityProfile`` is a new mandatory parameter of the ``getObject...`` methods. If set to ``‘V2’``, only objects that are encrypted in 
V2-compatible format can be decrypted. Setting this parameter  to ``‘V2_AND_LEGACY’`` also allows objects encrypted in V1-compatible 
format to be decrypted. To support migration, set ``@SecurityProfile`` to ``‘V2_AND_LEGACY’``.  Use ``‘V2’`` only for new application development.

3. (optional) Include the ``@KmsAllowDecryptWithAnyCmk`` parameter in the ``S3EncryptionClientV2::getObject`` and ``S3EncryptionClientV2::getObjectAsync* methods.``
A new parameter has been added called ``@KmsAllowDecryptWithAnyCmk``. Setting this parameter to ``true`` enables decryption without supplying a KMS key. The default value is ``false``.

4. For decryption with a V2 client, if the ``@KmsAllowDecryptWithAnyCmk`` parameter isn’t set to ``true`` for the ``“getObject...”`` method 
calls, a ``kms-key-id`` must be supplied to the ``KmsMaterialsProviderV2`` constructor.

Migration Examples
==================

Example 1: Migrating to V2 Clients
----------------------------------

**Pre-migration**

.. code-block:: php

 use Aws\S3\Crypto\S3EncryptionClient;
 use Aws\S3\S3Client;
 
 $encryptionClient = new S3EncryptionClient(
     new S3Client([
         'profile' => 'default',
         'region' => 'us-east-1',
         'version' => 'latest',
     ])
 );

**Post-migration**

.. code-block:: php

 use Aws\S3\Crypto\S3EncryptionClientV2;
 use Aws\S3\S3Client;

 $encryptionClient = new S3EncryptionClientV2(
     new S3Client([
         'profile' => 'default',
         'region' => 'us-east-1',
         'version' => 'latest',
     ])
 );

Example 2: Using KMS with kms-key-id
------------------------------------

.. note:: These examples use imports and variables defined in Example 1. For example, ``$encryptionClient``.

**Pre-migration**

.. code-block:: php

 use Aws\Crypto\KmsMaterialsProvider;
 use Aws\Kms\KmsClient;

 $kmsKeyId = 'kms-key-id';
 $materialsProvider = new KmsMaterialsProvider(
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
 ];

 $encryptionClient->putObject([
     '@MaterialsProvider' => $materialsProvider,
     '@CipherOptions' => $cipherOptions,
     'Bucket' => $bucket,
     'Key' => $key,
     'Body' => fopen('file-to-encrypt.txt', 'r'), 
 ]);

 $result = $encryptionClient->getObject([
     '@MaterialsProvider' => $materialsProvider,
     '@CipherOptions' => $cipherOptions,
     'Bucket' => $bucket,
     'Key' => $key,
 ]);

**Post-migration**

.. code-block:: php

  use Aws\Crypto\KmsMaterialsProviderV2;
  use Aws\Kms\KmsClient;

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
  ];

  $encryptionClient->putObject([
      '@MaterialsProvider' => $materialsProvider,
      '@CipherOptions' => $cipherOptions,
      '@KmsEncryptionContext' => ['context-key' => 'context-value'],
      'Bucket' => $bucket,
      'Key' => $key,
      'Body' => fopen('file-to-encrypt.txt', 'r'),
  ]);
  $result = $encryptionClient->getObject([
      '@KmsAllowDecryptWithAnyCmk' => true,
      '@SecurityProfile' => 'V2_AND_LEGACY',
      '@MaterialsProvider' => $materialsProvider,
      '@CipherOptions' => $cipherOptions,
      'Bucket' => $bucket,
      'Key' => $key,
  ]);

