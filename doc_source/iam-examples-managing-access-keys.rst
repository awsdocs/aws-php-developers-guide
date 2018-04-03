.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

==========================
Managing |IAM| Access Keys
==========================

.. meta::
   :description: Create, delete, and get information about |IAM| access keys.
   :keywords: |IAMlong|, |sdk-php| examples

Users need their own access keys to make programmatic calls to AWS. To fill this need, you can create, modify, view, or rotate access keys (access key IDs and secret access keys) for |IAM| users. By default, when you create an access key, its status is Active, which means the user can use the access key for API calls.

The examples below show how to:

* Create a secret access key and corresponding access key ID using :aws-php-class:`CreateAccessKey <api-iam-2010-05-08.html#createaccesskey>`.
* Return information about the access key IDs associated with an |IAM| user using :aws-php-class:`ListAccessKeys <api-iam-2010-05-08.html#listaccesskeys>`.
* Retrieve information about when an access key was last used using :aws-php-class:`GetAccessKeyLastUsed <api-iam-2010-05-08.html#getaccesskeylastused>`.
* Change the status of an access key from Active to Inactive, or vice versa, using :aws-php-class:`UpdateAccessKey <api-iam-2010-05-08.html#updateaccesskey>`.
* Delete an access key pair associated with an |IAM| user using :aws-php-class:`DeleteAccessKey <api-iam-2010-05-08.html#deleteaccesskey>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials, as described in :doc:`guide_credentials`.

Create an Access Key
--------------------


**Imports**

.. literalinclude::  example_code/iam/CreateAccessKey.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/CreateAccessKey.php
   :lines: 31-45
   :language: php


List Access Keys
----------------

**Imports**

.. literalinclude::  example_code/iam/ListAccessKeys.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/ListAccessKeys.php
   :lines: 31-43
   :language: php


Get Info about Access Key's Last Usage
--------------------------------------

**Imports**

.. literalinclude::  example_code/iam/GetAccessKeyLastUsed.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/GetAccessKeyLastUsed.php
   :lines: 31-45
   :language: php

Update an Access Key
--------------------

**Imports**

.. literalinclude::  example_code/iam/UpdateAccessKey.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/UpdateAccessKey.php
   :lines: 31-47
   :language: php

Delete an Access Key
--------------------

**Imports**

.. literalinclude::  example_code/iam/DeleteAccessKey.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/DeleteAccessKey.php
   :lines: 31-46
   :language: php
