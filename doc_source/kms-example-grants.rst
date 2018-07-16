.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################
Working with Grants using |KMS| API and the |sdk-php| version 3
###############################################################

.. meta::
   :description: Use AWS KMS API to create, view, retire and revoke grants.
   :keywords: Amazon KMS code examples for PHP, PHP grants, PHP Key Management Service Grants

The following examples show how to:

* Create a Grant :aws-php-class:`CreateGrant </api-kms-2014-11-01.html#creategrant>`.
* View a Grant :aws-php-class:`ListGrants </api-kms-2014-11-01.html.html#listgrants>`.
* Retire a Grant :aws-php-class:`RetireGrant </api-kms-2014-11-01.html.html#retiregrant>`.
* Revoke a Grant :aws-php-class:`RevokeGrant </api-kms-2014-11-01.html.html#revokegrant>`.

.. include:: text/git-php-examples.txt

Create a Grant
==============

**Imports**

.. literalinclude::  example_code/kms/CreateGrant.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/CreateGrant.php
   :lines: 33-
   :language: php


View a Grant
============

**Imports**

.. literalinclude::  example_code/kms/ListGrants.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListGrants.php
   :lines: 33-
   :language: php

Retire a Grant
==============

**Imports**

.. literalinclude::  example_code/kms/RetireGrant.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/RetireGrant.php
   :lines: 30-42
   :language: php

Revoke a Grant
==============

**Imports**

.. literalinclude::  example_code/kms/RevokeGrant.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/RevokeGrant.php
   :lines: 30-42
   :language: php

