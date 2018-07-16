.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

################################################################
Working with Aliases using |KMS| API and the |sdk-php| version 3
################################################################

.. meta::
   :description: Use AWS KMS API to create, view, update and delete aliases.
   :keywords: Amazon KMS code examples for PHP, PHP alias, PHP Key Management Service Alias, Customer Master Key in PHP, CMK PHP

An alias is an optional display name for a :KMS-dg:`customer master key (CMK)<concepts.html#master_keys>`.

The following examples show how to:

* Create an Alias :aws-php-class:`CreateAlias </api-kms-2014-11-01.html#createalias>`.
* View an Alias :aws-php-class:`ListAliases </api-kms-2014-11-01.html.html#listaliases>`.
* Update an Alias :aws-php-class:`UpdateAlias </api-kms-2014-11-01.html.html#updatealias>`.
* Delete an Alias :aws-php-class:`DeleteAlias </api-kms-2014-11-01.html.html#deletealias>`.

.. include:: text/git-php-examples.txt

Create a Grant
==============

**Imports**

.. literalinclude::  example_code/kms/CreateAlias.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/CreateAlias.php
   :lines: 33-
   :language: php


View a Grant
============

**Imports**

.. literalinclude::  example_code/kms/ListAliases.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListAliases.php
   :lines: 33-
   :language: php

Retire a Grant
==============

**Imports**

.. literalinclude::  example_code/kms/UpdateAlias.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/UpdateAlias.php
   :lines: 30-42
   :language: php

Revoke a Grant
==============

**Imports**

.. literalinclude::  example_code/kms/DeleteAlias.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DeleteAlias.php
   :lines: 30-42
   :language: php

