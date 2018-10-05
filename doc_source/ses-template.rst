.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################################
Creating Custom Email Templates Using the |SES| API and the |sdk-php| Version 3
###############################################################################

.. meta::
   :description: Use the Amazon SES API to create and use email templates.
   :keywords: Amazon SES code examples for PHP, create email templates with PHP

|SESlong| (|SES|) enables you to send emails that are personalized for each recipient by using templates. Templates include a subject line and the text and HTML parts of the email body. The subject and body sections can also contain unique values that are personalized for each recipient.

For more information, see :SES-dg:`Sending Personalized Email Using the Amazon SES <send-personalized-email-api>` in the |SES-dg|.

The following examples show how to:

* Create an email template using :aws-php-class:`CreateTemplate <api-email-2010-12-01.html#createtemplate>`.
* List all email templates using :aws-php-class:`ListTemplates <api-email-2010-12-01.html#listtemplates>`.
* Retrieve an email template using :aws-php-class:`GetTemplate <api-email-2010-12-01.html#gettemplate>`.
* Update an email template using :aws-php-class:`UpdateTemplate <api-email-2010-12-01.html#updateTemplate>`.
* Remove an email template using :aws-php-class:`DeleteTemplate <api-email-2010-12-01.html#deletetemplate>`.
* Send a templated email using :aws-php-class:`SendTemplatedEmail <api-email-2010-12-01.html#sendtemplatedemail>`.

.. include:: text/git-php-examples.txt

For more information about using |SES|, see the |SES-dg|_.

Create an Email Template
========================

To create a template to send personalized email messages, use the :SES-api:`CreateTemplate <API_CreateTemplate>` operation. The template can be used by any account authorized to send messages in the AWS Region to which the template is added.

.. note::
    |SES| doesn't validate your HTML, so be sure that `HtmlPart` is valid before sending an email.

**Imports**

.. literalinclude::  example_code/ses/Create_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Template.php
   :lines: 26-
   :language: php

Get an Email Template
=====================

To view the content for an existing email template including the subject line, HTML body, and plain text, use the :SES-api:`GetTemplate <API_GetTemplate>` operation. Only TemplateName is required.

**Imports**

.. literalinclude::  example_code/ses/Get_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Get_Template.php
   :lines: 26-
   :language: php

List All Email Templates
========================

To retrieve a list of all email templates that are associated with your AWS account in the current AWS Region, use the :SES-api:`ListTemplates <API_ListTemplates>` operation.

**Imports**

.. literalinclude::  example_code/ses/List_Templates.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Templates.php
   :lines: 26-
   :language: php

Update an Email Template
========================

To change the content for a specific email template including the subject line, HTML body, and plain text, use the :SES-api:`UpdateTemplate <API_UpdadteTemplate>` operation.

**Imports**

.. literalinclude::  example_code/ses/Update_Template.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Update_Template.php
   :lines: 26-
   :language: php

Delete an Email Template
========================

To remove a specific email template, use the :SES-api:`DeleteTemplate <API_DeleteTemplate>` operation. All you need is the TemplateName.

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

To use a template to send an email to recipients, use the :SES-api:`SendTemplatedEmail <API_SendTemplatedEmail>` operation.

**Imports**

.. literalinclude::  example_code/ses/Send_Templated_Email.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Send_Templated_Email.php
   :lines: 26-
   :language: php
