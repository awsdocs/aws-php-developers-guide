.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################################################
Encrypting and Decrypting |KMS| Data Keys using the |sdk-php| version 3
#######################################################################

.. meta::
   :description: Use |KMS| API to encrpypt and decrypt customer data keys.
   :keywords: Amazon KMS code examples for PHP, encrypt data in PHP, decypt data in PHP,

The following examples show how to:

* Encrypting a Key :aws-php-class:`Encrypt </api-kms-2014-11-01.html#encrypt>`.
* Decypting a Data Key :aws-php-class:`Decrypt </api-kms-2014-11-01.html.html#decrypt>`.
* Re-Encrypting a Data Key with new Master Key :aws-php-class:`ReEncrypt </api-kms-2014-11-01.html.html#reencrypt>`.


.. include:: text/git-php-examples.txt

For more information about using KMS, check out the |KMS-dg|_.

Encrypt
=======

The :KMS-api:`Encrypt <API_Encrypt>` operation is designed to encrypt data keys, but it is not frequently used. The :KMS-api:`GenerateDataKey <API_GenerateDataKey>` and 
:KMS-api:`GenerateDataKeyWithoutPlaintext <API_GenerateDataKeyWithoutPlaintext>` operations return encrypted data keys. You might use this 
method when you are moving encrypted data to a new region and want to encrypt its data key with a CMK in the new region.

**Imports**

.. literalinclude::  example_code/kms/encrypt.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/encrypt.php
   :lines: 33-
   :language: php

Decrypt
=======

To decrypt a data key, use the :KMS-api:`Decrypt <API_Decrypt>` operation.

The ciphertextBlob that you specify must be the value of the CiphertextBlob field from a :KMS-api:`GenerateDataKey <API_GenerateDataKey>`, 
:KMS-api:`GenerateDataKeyWithoutPlaintext <API_GenerateDataKeyWithoutPlaintext>`, or :KMS-api:`Encrypt <API_Encrypt>` response.


**Imports**

.. literalinclude::  example_code/kms/decrypt.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/decrypt.php
   :lines: 33-
   :language: php

ReEncrypt
=========

To decrypt an encrypted data key, and then immediately re-encrypt the data key under a different customer master key (CMK), use the 
:KMS-api:`ReEncrypt <API_ReEncrypt>` operation. The operations are performed entirely on the server side within |KMS|, so they never expose your plaintext outside of |KMS|.

The ciphertextBlob that you specify must be the value of the CiphertextBlob field from a :KMS-api:`GenerateDataKey <API_GenerateDataKey>`, 
:KMS-api:`GenerateDataKeyWithoutPlaintext <API_GenerateDataKeyWithoutPlaintext>`, or :KMS-api:`Encrypt <API_Encrypt>` response.

**Imports**

.. literalinclude::  example_code/kms/rencrypt.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/rencrypt.php
   :lines: 33-
   :language: php
