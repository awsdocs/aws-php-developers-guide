.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#####################################################
Working with |EC2| Key Pairs with |sdk-php| Version 3
#####################################################

.. meta::
   :description: Create and delete key pairs for Amazon EC2 using the AWS SDK for PHP version 3.
   :keywords: Amazon EC2 code examples for PHP

|EC2| uses public–key cryptography to encrypt and decrypt login information. Public–key cryptography uses
a public key to encrypt data. Then the recipient uses the private key to decrypt the data. The public and
private keys are known as a key pair.

The following examples show how to:

* Create a 2048-bit RSA key pair using :aws-php-class:`CreateKeyPair </api-ec2-2016-11-15.html#createkeypair>`.
* Delete a specified key pair using :aws-php-class:`DeleteKeyPair </api-ec2-2016-11-15.html#deletekeypair>`.
* Describe one or more of your key pairs using :aws-php-class:`DescribeKeyPairs </api-ec2-2016-11-15.html#describekeypairs>`.

.. include:: text/git-php-examples.txt

Create a Key Pair
=================

**Imports**

.. literalinclude::  example_code/ec2/CreateKeyPair.php
   :lines: 19-21
   :language: php

**Sample Code**

.. literalinclude:: example_code/ec2/CreateKeyPair.php
   :lines: 30-47
   :language: php


Delete a Key Pair
=================

**Imports**

.. literalinclude::  example_code/ec2/DeleteKeypair.php
   :lines: 19-21
   :language: php

**Sample Code**

.. literalinclude:: example_code/ec2/DeleteKeypair.php
   :lines: 30-43
   :language: php


Describe Key Pairs
==================

**Imports**

.. literalinclude::  example_code/ec2/DescribeKeyPairs.php
   :lines: 19-21
   :language: php

**Sample Code**

.. literalinclude:: example_code/ec2/DescribeKeyPairs.php
   :lines: 30-38
   :language: php
