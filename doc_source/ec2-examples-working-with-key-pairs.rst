.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

* Create a 2048-bit RSA key pair using :aws-php-class:`CreateKeyPair <api-ec2-2016-11-15.html#createkeypair>`.
* Delete a specified key pair using :aws-php-class:`DeleteKeyPair <api-ec2-2016-11-15.html#deletekeypair>`.
* Describe one or more of your key pairs using :aws-php-class:`DescribeKeyPairs <api-ec2-2016-11-15.html#describekeypairs>`.

.. include:: text/git-php-examples.txt

Create a Key Pair
=================

**Imports**

.. literalinclude:: ec2.php.create_key_pair.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.create_key_pair.main.txt
   :language: PHP

Delete a Key Pair
=================

**Imports**

.. literalinclude:: ec2.php.delete_key_pair.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.delete_key_pair.main.txt
   :language: PHP
   
Describe Key Pairs
==================

**Imports**

.. literalinclude:: ec2.php.describe_key_pairs.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.describe_key_pairs.main.txt
   :language: PHP
