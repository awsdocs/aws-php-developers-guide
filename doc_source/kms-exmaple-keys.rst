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

* Creating a Customer Master Key :aws-php-class:`CreateKey </api-kms-2014-11-01.html#createkey>`.
* Generating a Data Key:aws-php-class:`GenerateDataKey </api-kms-2014-11-01.html.html#generatedatakey>`.
* Viewing a Custom Master Key :aws-php-class:`DescribeKey </api-kms-2014-11-01.html.html#describekey>`.
* Getting Key IDs and Key ARNS of Customer Master Keys :aws-php-class:`ListKeys </api-kms-2014-11-01.html.html#listkeys>`.
* Enabling Customer Master Keys :aws-php-class:`EnableKey </api-kms-2014-11-01.html.html#enablekey>`.
* Disabling Customer Master Keys :aws-php-class:`DisableKey </api-kms-2014-11-01.html.html#disablekey>`.

.. include:: text/git-php-examples.txt

Create Key
==========

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

**Imports**

.. literalinclude::  example_code/kms/DisableKey.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DisableKey.php
   :lines: 30-42
   :language: php
