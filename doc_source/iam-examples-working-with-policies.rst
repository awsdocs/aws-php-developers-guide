.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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
* Attach a policy to a group using :aws-php-class:`AttachGroupPolicy <api-iam-2010-05-08.html#attachgrouppolicy>`.
* Remove a role policy using :aws-php-class:`DetachRolePolicy <api-iam-2010-05-08.html#detachrolepolicy>`.
* Remove a user policy using :aws-php-class:`DetachUserPolicy <api-iam-2010-05-08.html#detachuserpolicy>`.
* Remove a group policy using :aws-php-class:`DetachGroupPolicy <api-iam-2010-05-08.html#detachgrouppolicy>`.
* Delete a managed policy using :aws-php-class:`DeletePolicy <api-iam-2010-05-08.html#deletepolicy>`.
* Delete a role policy using :aws-php-class:`DeleteRolePolicy <api-iam-2010-05-08.html#deleterolepolicy>`.
* Delete a user policy using :aws-php-class:`DeleteUserPolicy <api-iam-2010-05-08.html#deleteuserpolicy>`.
* Delete a group policy using :aws-php-class:`DeleteGroupPolicy <api-iam-2010-05-08.html#deletegrouppolicy>`.

.. include:: text/git-php-examples.txt

Create a Policy
===============

**Imports**

.. literalinclude::  iam.php.create_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.create_policy.main.txt
   :language: PHP

Attach a Policy to a Role
=========================

**Imports**

.. literalinclude::  iam.php.attach_role_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.attach_role_policy.main.txt
   :language: PHP

Attach a Policy to a User
=========================

**Imports**

.. literalinclude::  iam.php.attach_user_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.attach_user_policy.main.txt
   :language: PHP
   
Attach a Policy to a Group
==========================

**Imports**

.. literalinclude::  iam.php.attach_group_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.attach_group_policy.main.txt
   :language: PHP

Detach a Role Policy
====================

**Imports**

.. literalinclude::  iam.php.detach_role_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.detach_role_policy.main.txt
   :language: PHP

Detach a User Policy
====================

**Imports**

.. literalinclude::  iam.php.detach_user_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.detach_user_policy.main.txt
   :language: PHP

Detach a Group Policy
=====================

**Imports**

.. literalinclude::  iam.php.detach_group_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.detach_group_policy.main.txt
   :language: PHP
   
Delete a Policy
===============

**Imports**

.. literalinclude::  iam.php.delete_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.delete_policy.main.txt
   :language: PHP

Delete a Role Policy
====================

**Imports**

.. literalinclude::  iam.php.delete_role_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.delete_role_policy.main.txt
   :language: PHP

Delete a User Policy
====================

**Imports**

.. literalinclude::  iam.php.delete_user_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.delete_user_policy.main.txt
   :language: PHP

Delete a Group Policy
=====================

**Imports**

.. literalinclude::  iam.php.delete_group_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: iam.php.delete_group_policy.main.txt
   :language: PHP