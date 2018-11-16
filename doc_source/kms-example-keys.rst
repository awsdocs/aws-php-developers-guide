.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################################
Working with Keys Using the |KMS| API and the |sdk-php| Version 3
#################################################################

.. meta::
   :description: Use AWS KMS API to create, view, enable and disable customer master keys.
   :keywords: AWS KMS code examples for PHP, master keys with PHP

The primary resources in |KMSlong| (|KMS|) are :KMS-dg:`customer master keys (CMKs)<concepts.html#master_keys>`. You can use a CMK to encrypt your data.

The following examples show how to:

* Create a customer CMK using :aws-php-class:`CreateKey <api-kms-2014-11-01.html#createkey>`.
* Generate a data key using :aws-php-class:`GenerateDataKey <api-kms-2014-11-01.html#generatedatakey>`.
* View a CMK using :aws-php-class:`DescribeKey <api-kms-2014-11-01.html#describekey>`.
* Get key IDs and key ARNS of CMKs using :aws-php-class:`ListKeys <api-kms-2014-11-01.html#listkeys>`.
* Enable CMKs using :aws-php-class:`EnableKey <api-kms-2014-11-01.html#enablekey>`.
* Disable CMKs using :aws-php-class:`DisableKey <api-kms-2014-11-01.html#disablekey>`.

.. include:: text/git-php-examples.txt

For more information about using |KMSlong| (|KMS|), see the |KMS-dg|_.

Create a CMK
============

To create a :KMS-dg:`CMK <concepts.html#master_keys>`, use the :KMS-api:`CreateKey <API_CreateKey>` operation.

**Imports**

.. literalinclude::  example_code/kms/CreateKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/CreateKey.php
   :lines: 33-
   :language: php

Generate a Data Key
===================

To generate a data encryption key, use the :KMS-api:`GenerateDataKey <API_GenerateDataKey>` operation. This operation returns plaintext and encrypted copies of the data key that it creates. You must specify the customer master key (CMK) under which to generate the data key.

**Imports**

.. literalinclude::  example_code/kms/GenerateDataKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/GenerateDataKey.php
   :lines: 33-
   :language: php

View a CMK
==========

To get detailed information about a CMK, including the CMK's Amazon Resource Name (ARN) and :KMS-dg:`key state<key-state>`, use the :KMS-api:`DescribeKey <API_DescribeKey>` operation.

:code:`DescribeKey` doesn't get aliases. To get aliases, use the :KMS-api:`ListAliases <API_ListKeys>` operation.

**Imports**

.. literalinclude::  example_code/kms/DescribeKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DescribeKey.php
   :lines: 33-
   :language: php

Get the Key ID and Key ARNs of a CMK
====================================

To get the ID and ARN of the CMK, use the :KMS-api:`ListAliases <API_ListKeys>` operation.

**Imports**

.. literalinclude::  example_code/kms/ListKeys.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListKeys.php
   :lines: 33-
   :language: php

Enable a CMK
============

To enable a disabled CMK, use the :KMS-api:`EnableKey <API_EnableKey>` operation.

**Imports**

.. literalinclude::  example_code/kms/EnableKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/EnableKey.php
   :lines: 33-
   :language: php

Disable a CMK
=============

To disable a CMK, use the :KMS-api:`DisableKey <API_DisableKey>` operation. Disabling a CMK prevents it from being used.

**Imports**

.. literalinclude::  example_code/kms/DisableKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DisableKey.php
   :lines: 33-
   :language: php
