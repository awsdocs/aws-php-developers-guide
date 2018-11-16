.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################################################
Creating and Managing Email Rules Using the |SES| API and the |sdk-php| Version 3
#################################################################################

.. meta::
   :description: Use the Amazon SES API to manage email rules.
   :keywords: Amazon SES code examples for PHP, managing email rules with PHP

In addition to sending emails, you can also receive email with |SESlong| (|SES|). Receipt rules enable you to specify what |SES| does with email it receives for the email addresses or domains you own.
A rule can send email to other AWS services including but not limited to |S3|, |SNS|, or |LAMlong|.

For more information, see :SES-dg:`Managing Receipt Rule Sets for Amazon SES Email Receiving <receiving-email-managing-receipt-rule-sets>` and :SES-dg:`Managing Receipt Rules for Amazon SES Email Receiving <receiving-email-managing-receipt-rules>`.

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

For more information about using |SES|, see the |SES-dg|_.

Create a Receipt Rule Set
==========================

A receipt rule set contains a collection of receipt rules. You must have at least one receipt rule set associated with your account before you can create a receipt rule. To create a receipt rule set, provide a unique RuleSetName and use the :SES-api:`CreateReceiptRuleSet <API_CreateReceiptRuleSet>` operation.

**Imports**

.. literalinclude::  example_code/ses/Create_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Rule_Set.php
   :lines: 26-
   :language: php

Create a Receipt Rule
=====================

Control your incoming email by adding a receipt rule to an existing receipt rule set. This example shows you how to create a receipt rule that sends incoming messages to an |S3| bucket, but you can also send messages to |SNS| and |LAMlong|. To create a receipt rule, provide a rule and the RuleSetName to the :SES-api:`CreateReceiptRule <API_CreateReceiptRule>` operation.

**Imports**

.. literalinclude::  example_code/ses/Create_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Rule.php
   :lines: 26-
   :language: php

Describe a Receipt Rule Set
============================

Once per second, return the details of the specified receipt rule set.
To use the :SES-api:`DescribeReceiptRuleSet <API_DescribeReceiptRuleSet>` operation, provide the RuleSetName.

**Imports**

.. literalinclude::  example_code/ses/Describe_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Describe_Rule_Set.php
   :lines: 26-
   :language: php

Describe a Receipt Rule
========================

Return the details of a specified receipt rule. To use the :SES-api:`DescribeReceiptRule <API_DescribeReceiptRule>` operation, provide the RuleName and RuleSetName.

**Imports**

.. literalinclude::  example_code/ses/Describe_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Describe_Rule.php
   :lines: 26-
   :language: php

List All Receipt Rule Sets
==========================

To list the receipt rule sets that exist under your AWS account in the current AWS Region, use the :SES-api:`ListReceiptRuleSets <API_ListReceiptRuleSets>` operation.

**Imports**

.. literalinclude::  example_code/ses/List_Rules_Sets.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Rules_Sets.php
   :lines: 26-
   :language: php

Update a Receipt Rule
=======================

This example shows you how to update a receipt rule that sends incoming messages to an |LAMlong| function, but you can also send messages to |SNS| and |S3|. To use the :SES-api:`UpdateReceiptRule <API_UpdateReceiptRule>` operation, provide the new receipt rule and the RuleSetName.

**Imports**

.. literalinclude::  example_code/ses/Update_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Update_Rule.php
   :lines: 26-
   :language: php

Delete a Receipt Rule Set
==========================

Remove a specified receipt rule set that isn't currently disabled. This also deletes all of the receipt rules it contains. To delete a receipt rule set, provide the RuleSetName to the :SES-api:`DeleteReceiptRuleSet <API_DeleteReceiptRuleSet>` operation.

**Imports**

.. literalinclude::  example_code/ses/Delete_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule_Set.php
   :lines: 26-
   :language: php

Delete a Receipt Rule
=====================

To delete a specified receipt rule, provide the RuleName and RuleSetName to the :SES-api:`DeleteReceiptRule <API_DeleteReceiptRule>` operation.

**Imports**

.. literalinclude::  example_code/ses/Delete_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule.php
   :lines: 26-
   :language: php
