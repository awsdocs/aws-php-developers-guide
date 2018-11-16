.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#############################################################
Working with |KMS| Key Policies Using the |sdk-php| Version 3
#############################################################

.. meta::
   :description: Use the AWS KMS API to view and change the key policies of AWS KMS customer master keys (CMKs).
   :keywords: AWS KMS code examples for PHP, PHP key polices

When you create an |KMSlong| (|KMS|) :KMS-dg:`customer master key (CMK)<concepts.html#master_keys>`, you determine who can use and manage that CMK. These permissions are contained in a document called the key policy. You can use the key policy to add, remove, or modify permissions at any time for a customer-managed CMK, but you cannot edit the key policy for a CMK that's managed by AWS. For more information, see :KMS-dg:`Authentication and Access Control for AWS KMS<control-access>`.

The following examples show how to:

* List the names of key policies using :aws-php-class:`ListKeyPolicies <api-kms-2014-11-01.html#listkeypolicies>`.
* Get a key policy using :aws-php-class:`GetKeyPolicy <api-kms-2014-11-01.html#getkeypolicy>`.
* Set a key policy using :aws-php-class:`PutKeyPolicy <api-kms-2014-11-01.html#putkeypolicy>`.


.. include:: text/git-php-examples.txt

For more information about using |KMSlong| (|KMS|), see the |KMS-dg|_.

List All Key Policies
=====================

To get the names of key policies for a CMK, use the :code:`ListKeyPolicies` operation. The only key policy name it returns is the default name.

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

To get the key policy for a CMK, use the :code:`GetKeyPolicy` operation.

:code:`GetKeyPolicy` requires a policy name. Unless you created a key policy when you created the CMK, the only valid policy name is the default. Learn more about the :KMS-dg:`default key policy<key-policies.html#key-policy-default>`.

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

To establish or change a key policy for a CMK, use the :code:`PutKeyPolicy` operation.

:code:`PutKeyPolicy` requires a policy name. Unless you created a Key Policy when you created the CMK, the only valid policy name is the default. Learn more about the :KMS-dg:`Default Key Policy<key-policies.html#key-policy-default>`.

**Imports**

.. literalinclude::  example_code/kms/PutKeyPolicy.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/PutKeyPolicy.php
   :lines: 33-
   :language: php
