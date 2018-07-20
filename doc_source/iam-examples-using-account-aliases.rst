.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################################
Using |IAM| Account Aliases with |sdk-php| Version 3
####################################################

.. meta::
   :description: Create, list, and delete aliases for AWS account IDs using AWS Identity and Access Management (IAM).
   :keywords: AWS Identity and Access Management (IAM) code examples, AWS SDK for PHP version 3

If you want the URL for your sign-in page to contain your company name or other friendly identifier instead of your AWS account ID, you can create an alias for your AWS account ID. If you create an AWS account alias, your sign-in page URL changes to incorporate the alias.

The following examples show how to:

* Create an alias using :aws-php-class:`CreateAccountAlias </api-iam-2010-05-08.html#createaccountalias>`.
* List the alias associated with the AWS account using :aws-php-class:`ListAccountAliases </api-iam-2010-05-08.html#listaccountaliases>`.
* Delete an alias using :aws-php-class:`DeleteAccountAlias </api-iam-2010-05-08.html#deleteaccountalias>`.

.. include:: text/git-php-examples.txt

Create an Alias
===============

**Imports**

.. literalinclude::  example_code/iam/CreateAccountAlias.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/CreateAccountAlias.php
   :lines: 31-46
   :language: php

List Account Aliases
====================

**Imports**

.. literalinclude::  example_code/iam/ListAccountAliases.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/ListAccountAliases.php
   :lines: 31-43
   :language: php

Delete an Alias
===============

**Imports**

.. literalinclude::  example_code/iam/DeleteAccountAlias.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/DeleteAccountAlias.php
   :lines: 31-46
   :language: php
