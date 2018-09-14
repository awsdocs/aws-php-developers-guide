.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#########################################################################
Managing Email Identities Using the |SES| API and the |sdk-php| Version 3
#########################################################################

.. meta::
   :description: Use Amazon SES API to verify email addresses and domains.
   :keywords: Amazon SES code examples for PHP, approve emails recipients with PHP

When you first start using your |SESlong| account, all senders and recipients must be verified in the same AWS Region that you
will be sending emails. For more information about sending emails, see :SES-dg:`Sending Email with Amazon SES <sending-email>`.

The following examples show how to:

* Verify an email address using :aws-php-class:`VerifyEmailIdentity <api-email-2010-12-01.html#verifyemailidentity>`.
* Verify an email domain using :aws-php-class:`VerifyDomainIdentity <api-email-2010-12-01.html.html#verifydomainidentity>`.
* List all email addresses using :aws-php-class:`ListIdentities <api-email-2010-12-01.html.html#listidentities>`.
* List all email domains using :aws-php-class:`ListIdentities <api-email-2010-12-01.html.html#listidentities>`.
* Remove an email address using :aws-php-class:`DeleteIdentity <api-email-2010-12-01.html.html#deleteidentity>`.
* Remove an email domain using :aws-php-class:`DeleteIdentity <api-email-2010-12-01.html.html#deleteidentity>`.

.. include:: text/git-php-examples.txt

For more information about using |SES|, see the |SES-dg|_.

Verify an Email Address
=======================
|SES| can only send email from verified email addresses or domains. By verifying an email address, you demonstrate that you're the owner of that address, and that you want to allow |SES| to send email from that address.

When you run the following code example, |SES| sends an email to the address you specified. When you (or the recipient of the email) click the link in the email, the address is verified.

**Imports**

.. literalinclude::  example_code/ses/Add_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Add_Email.php
   :lines: 26-
   :language: php

Verify an Email Domain
======================

|SES| can only send email from verified email addresses or domains. By verifying a domain, you demonstrate that you're the owner of that domain. When you verify a domain, you allow |SES| to send email from any address on that domain.

When you run the following code example, |SES| provides you with a verification token. You have to add the token to your domain's DNS configuration. For more information, see :SES-dg:`Verifying a Domain with Amazon SES <verify-domain-procedure>` in the |SES-dg|.

**Imports**

.. literalinclude::  example_code/ses/Add_Domain.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Add_Domain.php
   :lines: 26-
   :language: php

List Email Addresses
====================

Retrieve a list of email addresses submitted in the current AWS Region, regardless of verification status.

**Imports**

.. literalinclude::  example_code/ses/List_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Email.php
   :lines: 26-
   :language: php

List Email Domains
==================

Retrieve a list of email domains submitted in the current AWS Region, regardless of verification status.

**Imports**

.. literalinclude::  example_code/ses/List_Domain.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Domain.php
   :lines: 26-
   :language: php

Delete an Email Address
========================

Delete a verified email address from the list of identities.

**Imports**

.. literalinclude::  example_code/ses/Delete_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Email.php
   :lines: 26-
   :language: php

Delete an Email Domain
======================

Delete a verified email domain from the list of verified identities.

**Imports**

.. literalinclude::  example_code/ses/Delete_Domain.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Domain.php
   :lines: 26-
   :language: php
