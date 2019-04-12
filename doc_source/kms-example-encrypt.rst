.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################################################
Encrypting and Decrypting |KMS| Data Keys Using the |sdk-php| Version 3
#######################################################################

.. meta::
   :description: Use the AWS KMS API to encrypt and decrypt data.
   :keywords: AWS KMS code examples for PHP, encrypt data in PHP, decrypt data in PHP

Data keys are encryption keys that you can use to encrypt data, including large amounts of data and other data encryption keys.

You can use a |KMSlong| (|KMS|) :KMS-dg:`customer master key (CMK)<concepts.html#master_keys>` to generate, encrypt, and decrypt data keys. However, |KMS| does not store, manage, or track your data keys, or perform cryptographic operations with data keys. You must use and manage data keys outside of |KMS|.

The following examples show how to:

* Encrypt a data key using :aws-php-class:`Encrypt <api-kms-2014-11-01.html#encrypt>`.
* Decrypt a data key using :aws-php-class:`Decrypt <api-kms-2014-11-01.html#decrypt>`.
* Re-encrypt a data key with a new CMK using :aws-php-class:`ReEncrypt <api-kms-2014-11-01.html#reencrypt>`.


.. include:: text/git-php-examples.txt

For more information about using |KMSlong| (|KMS|), see the |KMS-dg|_.

Encrypt
=======

The :KMS-api:`Encrypt <API_Encrypt>` operation is designed to encrypt data keys, but it's not frequently used. The :KMS-api:`GenerateDataKey <API_GenerateDataKey>` and
:KMS-api:`GenerateDataKeyWithoutPlaintext <API_GenerateDataKeyWithoutPlaintext>` operations return encrypted data keys. You might use the ``Encypt``
method when you're moving encrypted data to a new AWS Region and want to encrypt its data key by using a CMK in the new Region.

**Imports**

.. literalinclude::  kms.php.encrypt.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.encrypt.main.txt
   :language: PHP

Decrypt
=======

To decrypt a data key, use the :KMS-api:`Decrypt <API_Decrypt>` operation.

The ``ciphertextBlob`` that you specify must be the value of the ``CiphertextBlob`` field from a :KMS-api:`GenerateDataKey <API_GenerateDataKey>`,
:KMS-api:`GenerateDataKeyWithoutPlaintext <API_GenerateDataKeyWithoutPlaintext>`, or :KMS-api:`Encrypt <API_Encrypt>` response.


**Imports**

.. literalinclude::  kms.php.decrypt.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.decrypt.main.txt
   :language: PHP

Reencrypt
=========

To decrypt an encrypted data key, and then immediately reencrypt the data key under a different CMK, use the
:KMS-api:`ReEncrypt <API_ReEncrypt>` operation. The operations are performed entirely on the server side within |KMS|, so they never expose your plaintext outside of |KMS|.

The ``ciphertextBlob`` that you specify must be the value of the ``CiphertextBlob`` field from a :KMS-api:`GenerateDataKey <API_GenerateDataKey>`,
:KMS-api:`GenerateDataKeyWithoutPlaintext <API_GenerateDataKeyWithoutPlaintext>`, or :KMS-api:`Encrypt <API_Encrypt>` response.

**Imports**

.. literalinclude::  kms.php.reencrypt.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.reencrypt.main.txt
   :language: PHP
