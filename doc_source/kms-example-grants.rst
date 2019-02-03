.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###################################################################
Working with Grants Using the |KMS| API and the |sdk-php| Version 3
###################################################################

.. meta::
   :description: Use AWS KMS API to create, view, retire, and revoke grants.
   :keywords: AWS KMS code examples for PHP, PHP grants, PHP Key Management Service Grants

A grant is another mechanism for providing permissions, an alternative to the key policy. You can use grants to give long-term access that allows AWS principals to use your |KMSlong| (|KMS|) :KMS-dg:`customer-managed CMKs<concepts.html#master_keys>`. For more information, see :KMS-dg:`Using Grants<grants>`.

The following examples show how to:

* Create a grant for a customer master key (CMK) using :aws-php-class:`CreateGrant <api-kms-2014-11-01.html#creategrant>`.
* View a grant for a CMK using :aws-php-class:`ListGrants <api-kms-2014-11-01.html#listgrants>`.
* Retire a grant for a CMK using :aws-php-class:`RetireGrant <api-kms-2014-11-01.html#retiregrant>`.
* Revoke a grant for a CMK using :aws-php-class:`RevokeGrant <api-kms-2014-11-01.html#revokegrant>`.

.. include:: text/git-php-examples.txt

For more information about using |KMSlong| (|KMS|), see the |KMS-dg|_.

Create a Grant
==============

To create a grant for an |KMS| CMK, use the :KMS-api:`CreateGrant <API_CreateGrant>` operation.

**Imports**

.. literalinclude::  kms.php.create_grant.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.create_grant.main.txt
   :language: PHP

View a Grant
============

To get detailed information about the grants on an |KMS| CMK, use the :KMS-api:`ListGrants <API_ListGrants>` operation.


**Imports**

.. literalinclude::  kms.php.list_grants.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.list_grants.main.txt
   :language: PHP

Retire a Grant
==============

To retire a grant for an |KMS| CMK, use the :KMS-api:`RetireGrant <API_RetireGrant>` operation. You should retire a grant to clean up after you finish using it.

**Imports**

.. literalinclude::  kms.php.retire_grant.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.retire_grant.main.txt
   :language: PHP

Revoke a Grant
==============

To revoke a grant to an |KMS| CMK, use the :KMS-api:`RevokeGrant <API_RevokeGrant>` operation. You can revoke a grant to explicitly deny operations that depend on it.

**Imports**

.. literalinclude::  kms.php.revoke_grant.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: kms.php.revoke_grant.main.txt
   :language: PHP
