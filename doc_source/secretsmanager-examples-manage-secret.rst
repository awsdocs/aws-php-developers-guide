.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################################################################
Managing Secrets stored in |ASMlong| Using the |ASM| API and the |sdk-php| Version 3
####################################################################################

.. meta::
   :description: Amazon Secrets Manager code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Secrets Manager code examples for PHP, Amazon Secrets Manager Secret

|ASMlong| Description


The following examples show how to:

* Create a new secret using :aws-php-class:`CreateSecret <api-secretsmanager-2017-10-17.html#createsecret>`.
* Retrieve a secret using :aws-php-class:`GetSecretValue <api-secretsmanager-2017-10-17.html#getsecretvalue>`.
* List all of the secrets stored by Secrets Manager using :aws-php-class:`ListSecrets <api-secretsmanager-2017-10-17.html#listsecrets>`.
* Get details about a specified secret using :aws-php-class:`DescribeSecret <api-secretsmanager-2017-10-17.html#describesecret>`.
* Update a specified secret using :aws-php-class:`PutSecretValue <api-secretsmanager-2017-10-17.html#putsecretvalue>`.
* Mark a secret for deletion  using :aws-php-class:`DeleteSecret <api-secretsmanager-2017-10-17.html#deletesecret>`.

.. include:: text/git-php-examples.txt

For more information about using |ASMlong|, see the |ASM-ug|_.

Create a secret in a |ASM|
==========================

To create a new secret in |ASM|, use the :ASM-api:`CreateSecret <API_CreateSecret>` operation.

**Imports**

.. literalinclude::  secretsmanager.php.create_secret.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.create_secret.main.txt
   :language: PHP

Retrieve a secret from |ASM| 
============================

To retrieve the value of a secret stored in |ASM|, use the :ASM-api:`GetSecretValue <API_GetSecretValue>` operation.

**Imports**

.. literalinclude:: secretsmanager.php.get_secret_value.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.get_secret_value.main.txt
   :language: php

List secrets stored in the |ASM|
================================

Get a list of all the secrets that are stored by |ASM| using the :ASM-api:`ListSecrets  <API_ListSecrets>` operation.

**Imports**

.. literalinclude:: secretsmanager.php.listsecrets.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.listsecrets.main.txt
   :language: php


Retrieve details about a secret  
===============================

To get the details of a specified secret stored in |ASM|, use the :ASM-api:`DescribeSecret <API_DescribeSecret>` operation.

**Imports**

.. literalinclude:: secretsmanager.php.describe_secret.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.describe_secret.main.txt
   :language: php

Update the secret value 
=======================

To update the value of a secret stored in |ASM|, use the :ASM-api:`PutSecretValue <API_PutSecretValue>` operation.

**Imports**

.. literalinclude:: secretsmanager.php.add_new_secret_value.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.add_new_secret_value.main.txt
   :language: php
   
Rotate the secret value in |ASM| 
================================

To update the value of a secret stored in |ASM|, use the :ASM-api:`PutSecretValue <API_PutSecretValue>` operation.

**Imports**

.. literalinclude:: secretsmanager.php.rotate_secret.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.rotate_secret.main.txt
   :language: php

Delete a secret from |ASM| 
==========================

To remove a specified secret from |ASM|, use the :ASM-api:`DeleteSecret <API_DeleteSecret>` operation.
A DeleteDate stamp will be added to the secret that specifics the length of the recovery window which is 30 days if not otherwise specified. After the recovery window has elapsed, the secret is deleted permanently. 

**Imports**

.. literalinclude:: secretsmanager.php.delete_secret.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.delete_secret.main.txt
   :language: php