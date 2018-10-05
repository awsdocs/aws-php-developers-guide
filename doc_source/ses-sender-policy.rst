.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###################################################################
Authorizing Senders Using the |SES| API and the |sdk-php| Version 3
###################################################################

.. meta::
   :description: Use the Amazon SES API to authorize other users to send emails from addresses or domains.
   :keywords: Amazon SES code examples for PHP, sending authorization with PHP

To enable another AWS account, |IAMlong| user, or AWS service to send email through |SESlong| (|SES|) on your behalf, you create a sending authorization policy. This is a JSON document that you attach to an identity that you own.

The policy expressly lists who you are allowing to send for that identity, and under which conditions. All senders, other than you and the entities you explicitly grant permissions to in the policy, are not allowed to send emails. An identity can have no policy, one policy, or multiple policies attached to it. You can also have one policy with multiple statements to achieve the effect of multiple policies.

For more information, see :SES-dg:`Using Sending Authorization with Amazon SES <sending-authorization>`.


The following examples show how to:

* Create an authorized sender using :aws-php-class:`PutIdentityPolicy  <api-email-2010-12-01.html#createidentitypolicy>`.
* Retrieve polices for an authorized sender using :aws-php-class:`GetIdentityPolicies <api-email-2010-12-01.html#getidentitypolicies>`.
* List authorized senders using :aws-php-class:`ListIdentityPolicies <api-email-2010-12-01.html#listidentitypolicies>`.
* Revoke permission for an authorized sender using :aws-php-class:`DeleteIdentityPolicy <api-email-2010-12-01.html#deleteidentitypolicy>`.

.. include:: text/git-php-examples.txt

For more information about using |SES|, see the |SES-dg|_.

Create an Authorized Sender
===========================

To authorize another AWS account to send emails on your behalf, use an identity policy to add or update authorization to send emails from your verified email addresses or domains. To create an identity policy, use the :SES-api:`PutIdentityPolicy <API_PutIdentityPolicy>` operation.

**Imports**

.. literalinclude::  example_code/ses/Authorize_Sender.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Authorize_Sender.php
   :lines: 26-
   :language: php



Retrieve Polices for an Authorized Sender
=========================================

Return the sending authorization policies that are associated with a specific email identity or domain identity. To get the sending authorization for a given email address or domain, use the :SES-api:`GetIdentityPolicy <API_GetIdentityPolicy>` operation.

**Imports**

.. literalinclude::  example_code/ses/Get_Authorized_Policies.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Get_Authorized_Policies.php
   :lines: 26-
   :language: php

List Authorized Senders
=======================

To list the sending authorization policies that are associated with a specific email identity or domain identity in the current AWS Region, use the :SES-api:`ListIdentityPolicies <API_ListIdentityPolicies>` operation.

**Imports**

.. literalinclude::  example_code/ses/List_Authorized_Senders.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Authorized_Senders.php
   :lines: 26-
   :language: php



Revoke Permission for an Authorized Sender
==========================================

Remove sending authorization for another AWS account to send emails with an email identity or domain identity by deleting the associated identity policy with the :SES-api:`DeleteIdentityPolicy <API_DeleteIdentityPolicy>` operation.

**Imports**

.. literalinclude::  example_code/ses/Delete_Authorized_Sender.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Authorized_Sender.php
   :lines: 26-
   :language: php
