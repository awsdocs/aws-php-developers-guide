.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#############################################################
Working with Keys using |KMS| API and the |sdk-php| version 3
#############################################################

.. meta::
   :description: Use AWS KMS API to create, view, enable and disable customer master keys.
   :keywords: Amazon KMS code examples for PHP, master keys with PHP

The following examples show how to:

* Creating a Customer Master Key :aws-php-class:`CreateKey <api-kms-2014-11-01.html#createkey>`.
* Generating a Data Key:aws-php-class:`GenerateDataKey <api-kms-2014-11-01.html.html#generatedatakey>`.
* Viewing a Custom Master Key :aws-php-class:`DescribeKey <api-kms-2014-11-01.html.html#describekey>`.
* Getting Key IDs and Key ARNS of Customer Master Keys :aws-php-class:`ListKeys <api-kms-2014-11-01.html.html#listkeys>`.
* Enabling Customer Master Keys :aws-php-class:`EnableKey <api-kms-2014-11-01.html.html#enablekey>`.
* Disabling Customer Master Keys :aws-php-class:`DisableKey <api-kms-2014-11-01.html.html#disablekey>`.

.. include:: text/git-php-examples.txt

For more information about using KMS, check out the |KMS-dg|_.

Create Key
==========

To create a :KMS-dg:`customer master key <concepts.html#master_keys>`, use the :KMS-api:`CreateKey <API_CreateKey>` operation.

**Imports**

.. literalinclude::  example_code/kms/CreateKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/CreateKey.php
   :lines: 33-
   :language: php

Generate Data Key
=================

To generate a data key, use the :KMS-api:`GenerateDataKey <API_GenerateDataKey>` operation. This operation returns plaintext and encrypted copies of the data key that it creates.

**Imports**

.. literalinclude::  example_code/kms/GenerateDataKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/GenerateDataKey.php
   :lines: 33-
   :language: php

View Custom Master Key
======================

To get detailed information about a customer master key (CMK), including the CMK ARN and :KMS-dg:`key state<key-state>`, use the :KMS-api:`DescribeKey <API_DescribeKey>` operation.

DescribeKey does not get aliases. To get aliases, use the :KMS-api:`ListAliases <API_ListKeys>` operation.

**Imports**

.. literalinclude::  example_code/kms/DescribeKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DescribeKey.php
   :lines: 33-
   :language: php

Get Key IDs and Key ARNs of Custom Master Key
=============================================

To get the IDs and ARNs of the customer master keys, use the :KMS-api:`ListAliases <API_ListKeys>` operation.

**Imports**

.. literalinclude::  example_code/kms/ListKeys.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListKeys.php
   :lines: 30-42
   :language: php

Enable Custom Master Key
=========================

To enable a disabled customer master key (CMK), use the :KMS-api:`EnableKey <API_EnableKey>` operation.

**Imports**

.. literalinclude::  example_code/kms/EnableKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/EnableKey.php
   :lines: 30-42
   :language: php

Disable Custom Master Key
=========================

To disable a CMK, use the :KMS-api:`DisableKey <API_DisableKey>` operation. Disabling a CMK prevents it from being used.

**Imports**

.. literalinclude::  example_code/kms/DisableKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DisableKey.php
   :lines: 30-42
   :language: php
