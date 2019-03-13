.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################################################################
Managing Secrets Stored in |ASMlong| Using the |ASM| API and the |sdk-php| Version 3
####################################################################################

.. meta::
   :description: Amazon Secrets Manager code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Secrets Manager code examples for PHP, Amazon Secrets Manager Secret

|ASMlong| stores and manages shared secrets such as passwords, API keys, or database credentials. With the |ASM| 
service, developers can replace hard coded credentials in deployed code with an embedded call to |ASM|. 

|ASM| natively supports automatic scheduled credential rotation for |RDS| databases, increasing application security. |ASM| can
also seamlessly rotate secrets for other databases and third-party services using |LAM| to implement service-specific details. 

The following examples show how to:

* Create a new secret using :aws-php-class:`CreateSecret <api-secretsmanager-2017-10-17.html#createsecret>`.
* Retrieve a secret using :aws-php-class:`GetSecretValue <api-secretsmanager-2017-10-17.html#getsecretvalue>`.
* List all of the secrets stored by Secrets Manager using :aws-php-class:`ListSecrets <api-secretsmanager-2017-10-17.html#listsecrets>`.
* Get details about a specified secret using :aws-php-class:`DescribeSecret <api-secretsmanager-2017-10-17.html#describesecret>`.
* Update a specified secret using :aws-php-class:`PutSecretValue <api-secretsmanager-2017-10-17.html#putsecretvalue>`.
* Set up a secret rotation using :aws-php-class:`RotateSecret <api-secretsmanager-2017-10-17.html#rotatesecret>`.
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
   

Rotate the value to an existing secret in |ASM| 
===============================================

To rotate the value of an existing secret stored in |ASM|, use a |LAM| rotation function and the :ASM-api:`RotateSecret <API_RotateSecret>` operation.

For more information about rotating secrets see  :ASM-ug:`Rotating Your AWS Secrets Manager Secrets <rotating-secrets>` in the |ASM-ug|.

Before you begin, create a |LAM| function to rotate your secret. We currently have several |LAM| code examples for rotating RDS database credentials in the 
S`AWS Code Sample Catalog <https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-lambda_functions-secretsmanager.html>`__ .

Once you set up your |LAM| function, configure a new secret rotation. 

**Imports**

.. literalinclude:: secretsmanager.php.create_secret_rotation.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.create_secret_rotation.main.txt
   :language: php

Once a rotation has been configured, you can implement a rotation using the :ASM-api:`RotateSecret <API_RotateSecret>` operation.

**Imports**

.. literalinclude:: secretsmanager.php.rotate_secret.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.rotate_secret.main.txt
   :language: php
   
Delete a secret from |ASM| 
==========================

To remove a specified secret from |ASM|, use the :ASM-api:`DeleteSecret <API_DeleteSecret>` operation.
To prevent accidental deletion, a DeleteDate stamp is added to the secret that specifics a recovery window where the secret's 
deletion can be reversed.  The recovery window is 30 days if not otherwise specified. After the recovery window has elapsed, 
the secret is deleted permanently. 

**Imports**

.. literalinclude:: secretsmanager.php.delete_secret.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: secretsmanager.php.delete_secret.main.txt
   :language: php