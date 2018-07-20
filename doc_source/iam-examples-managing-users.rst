.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#############################################
Managing |IAM| Users with |sdk-php| Version 3
#############################################

.. meta::
   :description: Create, list, update, or retrieve info about AWS Identity and Access Management (IAM) users.
   :keywords: AWS Identity and Access Management (IAM) code examples, AWS SDK for PHP version 3

An |IAM| user is an entity that you create in AWS to represent the person or service that uses it to interact with AWS. A user in AWS consists of a name and credentials.

The following examples show how to:

* Create a new |IAM| user using :aws-php-class:`CreateUser </api-iam-2010-05-08.html#createuser>`.
* List |IAM| users using :aws-php-class:`ListUsers </api-iam-2010-05-08.html#listusers>`.
* Update an |IAM| user using :aws-php-class:`UpdateUser </api-iam-2010-05-08.html#updateuser>`.
* Retrieve information about an |IAM| user using :aws-php-class:`GetUser </api-iam-2010-05-08.html#getuser>`.
* Delete an |IAM| user using :aws-php-class:`DeleteUser </api-iam-2010-05-08.html#deleteuser>`.

.. include:: text/git-php-examples.txt

Create an |IAM| User
====================

**Imports**

.. literalinclude::  example_code/iam/CreateUser.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/CreateUser.php
   :lines: 31-46
   :language: php


List |IAM| Users
================

**Imports**

.. literalinclude::  example_code/iam/ListUsers.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/ListUsers.php
   :lines: 31-43
   :language: php

Update an |IAM| User
====================

**Imports**

.. literalinclude::  example_code/iam/UpdateUser.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/UpdateUser.php
   :lines: 31-47
   :language: php


Get Information about an |IAM| User
===================================

**Imports**

.. literalinclude::  example_code/iam/GetUser.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/GetUser.php
   :lines: 31-45
   :language: php

Delete an |IAM| User
====================

**Imports**

.. literalinclude::  example_code/iam/DeleteUser.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/DeleteUser.php
   :lines: 31-46
   :language: php