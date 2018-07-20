.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################################
Working with |IAM| Policies with |sdk-php| Version 3
####################################################

.. meta::
   :description: Create, attach, or remove AWS Identity and Access Management (IAM) user policies.
   :keywords: AWS Identity and Access Management (IAM) code examples, AWS SDK for PHP version 3

You grant permissions to a user by creating a policy. A policy is a document that lists the actions that a user can perform and the resources those actions can affect. By default, any actions or resources that are not explicitly allowed are denied. Policies can be created and attached to users, groups of users, roles assumed by users, and resources.

The following examples show how to:

* Create a managed policy using :aws-php-class:`CreatePolicy <api-iam-2010-05-08.html#createpolicy>`.
* Attach a policy to a role using :aws-php-class:`AttachRolePolicy <api-iam-2010-05-08.html#attachrolepolicy>`.
* Attach a policy to a user using :aws-php-class:`AttachUserPolicy <api-iam-2010-05-08.html#attachuserpolicy>`.
* Remove a user policy using :aws-php-class:`DetachUserPolicy <api-iam-2010-05-08.html#detachuserpolicy>`.

.. include:: text/git-php-examples.txt


Create a Policy
===============

**Imports**

.. literalinclude::  example_code/iam/CreatePolicy.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/CreatePolicy.php
   :lines: 31-70
   :language: php

Attach a Policy to a Role
=========================

**Imports**

.. literalinclude::  example_code/iam/AttachRolePolicy.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/AttachRolePolicy.php
   :lines: 31-65
   :language: php

Attach a Policy to a User
=========================

**Imports**

.. literalinclude::  example_code/iam/AttachUserPolicy.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/AttachUserPolicy.php
   :lines: 31-65
   :language: php

Detach a User Policy
====================

**Imports**

.. literalinclude::  example_code/iam/DetachUserPolicy.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/DetachUserPolicy.php
   :lines: 31-49
   :language: php
