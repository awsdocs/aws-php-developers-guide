.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################################
Verify Email Addresses Using the |SESlong| API and the |sdk-php| Version 3
##########################################################################

.. meta::
   :description: Use AWS SES API to verify email address and domains.
   :keywords: AWS SES code examples for PHP, approve emails recipeints with PHP

When you first start using your Amazon SES account, all senders and recipeints must be verified in the same domain that you 
will be sending emails. For more information about sending emails see :SES-dg:`Sending Email with Amazon SES <sending-email.html>`


The following examples show how to:

* Verify an email address using :aws-php-class:`CreateKey <api-ses-2014-11-01.html#createkey>`.
* Verify an email domain using :aws-php-class:`GenerateDataKey <api-ses-2014-11-01.html.html#generatedatakey>`.
* Remove an email address using :aws-php-class:`DescribeKey <api-ses-2014-11-01.html.html#describekey>`.
* Remove an email domain using :aws-php-class:`ListKeys <api-ses-2014-11-01.html.html#listkeys>`.

.. include:: text/git-php-examples.txt

For more information about using |SESlong|, see the |SES-dg|_.

Verify an Email Identity
========================

To send or recieve an email address, you must authorize that email's identity. Using this code sample will send an
email to the address, where the recipeint must click a link to authorize |SESlong| to send or recieve messages to this address. 

**Imports**

.. literalinclude::  example_code/ses/Add_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Add_Email.php
   :lines: 33-
   :language: php

Verify an Email Domain
========================

To send or recieve an email address, you must authorize that email's identity. Using this code sample will whitelist an entire email domain  to authorize |SESlong| to send or recieve messages to this address. 

**Imports**

.. literalinclude::  example_code/ses/Add_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Add_Email.php
   :lines: 33-
   :language: php

      