.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################################################
Create and Manage Email Rules Using the |SESlong| API and the |sdk-php| Version 3
#################################################################################

.. meta::
   :description: Use AWS SES API to verify email address and domains.
   :keywords: AWS SES code examples for PHP, IP Address Email Filters with PHP

In addition to sending emails, you can also recieve email with |SESlong|. AReceipt rules let you specify what Amazon SES does with email it receives for the email addresses or domains you own. 
A rule can send email to other AWS services inlcuding but not limited to |S3|, |SNS|, or |LAMlong|.

For more information about recieving emails see :SES-dg:`Managing Receipt Rule Sets for Amazon SES Email Receiving <receiving-email-managing-receipt-rule-sets>` and:SES-dg:`Managing Receipt Rules for Amazon SES Email Receiving <receiving-email-managing-receipt-rules>`


The following examples show how to:

* Create a receipt rule set using :aws-php-class:`CreateReceiptRuleSet  <api-email-2010-12-01.html#createreceiptruleset>`.
* Create a receipt rule using :aws-php-class:`CreateReceiptRule <api-email-2010-12-01.html#createreceiptrule>`.
* Describe a receipt rule set using :aws-php-class:`DescribeReceiptRuleSet <api-email-2010-12-01.html#describereceiptruleset>`.
* Describe a receipt rule using :aws-php-class:`DescribeReceiptRule  <api-email-2010-12-01.html#describereceiptrule>`.
* List all receipt rule sets using :aws-php-class:`ListReceiptRuleSets <api-email-2010-12-01.html#listreceiptrulesets>`.
* Update a receipt rule using :aws-php-class:`UpdateReceiptRule <api-email-2010-12-01.html#updatereceiptrule>`.
* Remove a receipt rule using :aws-php-class:`DeleteReceiptRule <api-email-2010-12-01.html#deletereceiptrule>`.
* Remove a receipt rule set using :aws-php-class:`DeleteReceiptRuleSet <api-email-2010-12-01.html#deletereceiptruleset>`.

.. include:: text/git-php-examples.txt

For more information about using |SESlong|, see the |SES-dg|_.

Create an Receipt Rule Set
==========================

First, create a reciept rule set to contain a collection of receipt rules.  You must have at least one receipt rule set associated with your account before you can create a rule.  

**Imports**

.. literalinclude::  example_code/ses/Create_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Rule_Set.php
   :lines: 26-
   :language: php

Create an Receipt Rule 
=======================

Control your incomming email by adding a receipt rule to an exisiting receipt rule set 

**Imports**

.. literalinclude::  example_code/ses/Create_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Rule.php
   :lines: 26-
   :language: php

Describe an Receipt Rule Set
============================

Once per second, return the details of the specified receipt rule set.

**Imports**

.. literalinclude::  example_code/ses/Describe_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Describe_Rule_Set.php
   :lines: 26-
   :language: php

Describe an Receipt Rule 
========================

Returns the details of the specified receipt rule.

**Imports**

.. literalinclude::  example_code/ses/Describe_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Describe_Rule.php
   :lines: 26-
   :language: php
   
List all Receipt Rule Sets
==========================

Lists the receipt rule sets that exist under your AWS account in the current AWS Region.

**Imports**

.. literalinclude::  example_code/ses/List_Rule_Sets.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Rule_Sets.php
   :lines: 26-
   :language: php

Update an Receipt Rule 
=======================

Change an existing receipt rule.

**Imports**

.. literalinclude::  example_code/ses/Update_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Update_Rule.php
   :lines: 26-
   :language: php

Delete an Receipt Rule Set
==========================

Remove a specified receipt rule set that is not currently disabled. This will also delete all of the receipt rules it contains.

**Imports**

.. literalinclude::  example_code/ses/Delete_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule_Set.php
   :lines: 26-
   :language: php

Delete an Receipt Rule 
=======================

Deletes the specified receipt rule.

**Imports**

.. literalinclude::  example_code/ses/Delete_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule.php
   :lines: 26-
   :language: php
