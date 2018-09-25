.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################################################
Working with Aliases Using the |KMS| API and the |sdk-php| Version 3
####################################################################

.. meta::
   :description: Use AWS KMS API to create, view, update, and delete aliases.
   :keywords: AWS KMS code examples for PHP, PHP alias, PHP Key Management Service Alias, Customer Master Key in PHP, CMK PHP

An alias is an optional display name for an |KMSlong| (|KMS|) :KMS-dg:`customer master key (CMK)<concepts.html#master_keys>`.

The following examples show how to:

* Create an alias using :aws-php-class:`CreateAlias <api-kms-2014-11-01.html#createalias>`.
* View an alias using :aws-php-class:`ListAliases <api-kms-2014-11-01.html#listaliases>`.
* Update an alias using :aws-php-class:`UpdateAlias <api-kms-2014-11-01.html#updatealias>`.
* Delete an alias using :aws-php-class:`DeleteAlias <api-kms-2014-11-01.html#deletealias>`.

.. include:: text/git-php-examples.txt

For more information about using |KMSlong| (|KMS|), see the |KMS-dg|_.

Create an Alias
===============

To create an alias for a CMK, use the :KMS-api:`CreateAlias <API_CreateAlias>` operation. The alias must be unique in the account and AWS Region.
If you create an alias for a CMK that already has an alias, ``CreateAlias`` creates another alias to the same CMK. It doesn't replace the existing alias.


**Imports**

.. literalinclude::  example_code/kms/CreateAlias.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/CreateAlias.php
   :lines: 33-
   :language: php


View an Alias
=============

To list all aliases, use the :KMS-api:`ListAliases <API_ListAliases>` operation. The response includes aliases that are defined by AWS services, but are not associated with a CMK.


**Imports**

.. literalinclude::  example_code/kms/ListAliases.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListAliases.php
   :lines: 33-
   :language: php

Update an Alias
===============

To associate an existing alias with a different CMK, use the :KMS-api:`UpdateAlias <API_UpdateAlias>` operation.

**Imports**

.. literalinclude::  example_code/kms/UpdateAlias.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/UpdateAlias.php
   :lines: 33-
   :language: php

Delete an Alias
===============

To delete an alias, use the :KMS-api:`DeleteAlias <API_DeleteAlias>` operation. Deleting an alias has no effect on the underlying CMK.

**Imports**

.. literalinclude::  example_code/kms/DeleteAlias.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/DeleteAlias.php
   :lines: 33-
   :language: php
