.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

===========================
Working with |IAM| Policies
===========================

.. meta::
   :description: Create, attach, or remove |IAM| user policies.
   :keywords: |IAMlong| , |sdk-php| examples

You grant permissions to a user by creating a policy, which is a document that lists the actions that a user can perform and the resources those actions can affect. Any actions or resources that are not explicitly allowed are denied by default. Policies can be created and attached to users, groups of users, roles assumed by users, and resources.

The examples below show how to:

* Create a managed policy using :aws-php-class:`CreatePolicy <api-iam-2010-05-08.html#createpolicy>`.
* Attach a policy to a role using :aws-php-class:`AttachRolePolicy <api-iam-2010-05-08.html#attachrolepolicy>`.
* Attach a policy to a user using :aws-php-class:`AttachUserPolicy <api-iam-2010-05-08.html#attachuserpolicy>`.
* Remove a user policy using :aws-php-class:`DetachUserPolicy <api-iam-2010-05-08.html#detachuserpolicy>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials, as described in :doc:`guide_credentials`.

Create a Policy
---------------

**Imports**

.. literalinclude::  example_code/iam/CreatePolicy.php
   :lines: 15-18
   :language: php

**Code**

.. literalinclude:: example_code/iam/CreatePolicy.php
   :lines: 27-66
   :language: php

Attach a Policy to a Role
-------------------------

**Imports**

.. literalinclude::  example_code/iam/AttachRolePolicy.php
   :lines: 15-18
   :language: php

**Code**

.. literalinclude:: example_code/iam/AttachRolePolicy.php
   :lines: 27-61
   :language: php

Attach a Policy to a User
-------------------------

**Imports**

.. literalinclude::  example_code/iam/AttachUserPolicy.php
   :lines: 15-18
   :language: php

**Code**

.. literalinclude:: example_code/iam/AttachUserPolicy.php
   :lines: 27-61
   :language: php

Detach a User Policy
--------------------

**Imports**

.. literalinclude::  example_code/iam/DetachUserPolicy.php
   :lines: 15-18
   :language: php

**Code**

.. literalinclude:: example_code/iam/DetachUserPolicy.php
   :lines: 27-44
   :language: php
