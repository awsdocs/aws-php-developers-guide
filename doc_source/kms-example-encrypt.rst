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
   :description: Use AWS KMS API to encrpypt and decrypt customer data keys.
   :keywords: Amazon KMS code examples for PHP, encrypt data in PHP, decypt data in PHP,

The following examples show how to:

* Encrypting a Key :aws-php-class:`Encrypt </api-kms-2014-11-01.html#encrypt>`.
* Decypting a Data Key :aws-php-class:`Decrypt </api-kms-2014-11-01.html.html#decrypt>`.
* Re-Encrypting a Data Key with new Master Key :aws-php-class:`ReEncrypt </api-kms-2014-11-01.html.html#reencrypt>`.


.. include:: text/git-php-examples.txt

Encrypt
=======

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

**Imports**

.. literalinclude::  example_code/kms/rencrypt.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/rencrypt.php
   :lines: 33-
   :language: php
