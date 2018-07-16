.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#############################################################
Working with |KMS| Key Policies using the |sdk-php| version 3
#############################################################

.. meta::
   :description: Use AWS KMS API to view and change the key policies of AWS KMS customer master keys (CMKs).
   :keywords: Amazon KMS code examples for PHP, PHP Key Polices, PHP Key Policy

The following examples show how to:

* List Key Policy Names :aws-php-class:`ListKeyPolicies </api-kms-2014-11-01.html#listkeypolicies>`.
* Get a Key Policy :aws-php-class:`GetKeyPolicy </api-kms-2014-11-01.html.html#getkeypolicy>`.
* Set a Key Policy :aws-php-class:`PutKeyPolicy </api-kms-2014-11-01.html.html#putkeypolicy>`.


.. include:: text/git-php-examples.txt

List all Key Policies
=====================

**Imports**

.. literalinclude::  example_code/kms/ListKeyPolicies.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListKeyPolicies.php
   :lines: 33-
   :language: php

Retrieve a Key Policy
=====================


**Imports**

.. literalinclude::  example_code/kms/GetKeyPolicy.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/GetKeyPolicy.php
   :lines: 33-
   :language: php

Set a Key Policy
================

**Imports**

.. literalinclude::  example_code/kms/PutKeyPolicy.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/PutKeyPolicy.php
   :lines: 33-
   :language: php
