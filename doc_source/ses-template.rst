.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################################################
Create Custom Email Templates Using the |SESlong| API and the |sdk-php| Version 3
#################################################################################

.. meta::
   :description: Use AWS SES API to verify email address and domains.
   :keywords: AWS SES code examples for PHP, approve emails recipeints with PHP

|SESlong| allows you to send emails that are personalized for each recipeint using templates. Templates include a subject line, and the text and HTML parts of the email body. The subject and body sections may also contain unique values that are personalized for each recipient. For more information about sending emails see :SES-dg:`Sending Personalized Email Using the Amazon SES <send-personalized-email-api>` in the |SES-dg|.


The following examples show how to:

* Create an email template using :aws-php-class:`CreateTemplate <api-email-2010-12-01.html#createtemplate>`.
* List all email templates using :aws-php-class:`ListTemplates <api-email-2010-12-01.html.html#listtemplates>`.
* Retrieve an email template using :aws-php-class:`GetTemplate <api-email-2010-12-01.html.html#gettemplate>`.
* Update an email template using :aws-php-class:`UpdateTemplate <api-email-2010-12-01.html.html#updateTemplate>`.
* Remove an email template using :aws-php-class:`DeleteTemplate <api-email-2010-12-01.html.html#deletetemplate>`.
* Send a templated email using :aws-php-class:`SendTemplatedEmail <api-email-2010-12-01.html.html#sendtemplatedemail>`.

.. include:: text/git-php-examples.txt

For more information about using |SESlong|, see the |SES-dg|_.

Create an Email Template
========================

Create a new template to send personalized email messages. 

.. note:
    |SES| does not validate your HTML, so double check that `HtmlPart` is valid before sending an email. 

**Imports**

.. literalinclude::  example_code/ses/Create_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Template.php
   :lines: 26-
   :language: php
   
Get an Email Templates
========================

View the content for an existing email template including the subject line, HTML body, and plain text. 

**Imports**

.. literalinclude::  example_code/ses/Get_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Get_Template.php
   :lines: 26-
   :language: php

List all Email Templates
========================

Retrieve a list of all email templates associated with your AWS account in the current AWS Region.

**Imports**

.. literalinclude::  example_code/ses/List_Templates.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Templates.php
   :lines: 26-
   :language: php
   
Update an Email Templates
=========================

Change the content for a specific email template including the subject line, HTML body, and plain text. 

**Imports**

.. literalinclude::  example_code/ses/Get_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Get_Template.php
   :lines: 26-
   :language: php

Delete an Email Template
========================

Remove a specific email template. 

**Imports**

.. literalinclude::  example_code/ses/Delete_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Template.php
   :lines: 26-
   :language: php

Send an Email with a Template
=============================

Use a template to send an email to recipeints.

**Imports**

.. literalinclude::  example_code/ses/Send_Templated_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Send_Templated_Email.php
   :lines: 26-
   :language: php
