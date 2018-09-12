.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##################################################################################
Monitor your Sender Activity Using the |SESlong| API and the |sdk-php| Version 3
##################################################################################

.. meta::
   :description: Use the AWS SES API to monitor your sending reputation.
   :keywords: AWS SES code examples for PHP, Simple Email Service Quota with PHP, Simple Email Statistics with PHP

|SES| provides methods for monitoring your sending activity. We recommend that you implement these methods so that you can keep track of important measures, such as your account's bounce, complaint and reject rates. Excessively high bounce and complaint rates may jeopardize your ability to send emails using |SES|.

The following examples show how to:

* Check your sending quota using :aws-php-class:`GetSendQuota <api-email-2010-12-01.html#getsendquota>`.
* Monitor your sending activity using :aws-php-class:`GetSendStatistics <api-email-2010-12-01.html.html#getsendstatistics>`.

.. include:: text/git-php-examples.txt

For more information about using |SESlong|, see the |SES-dg|_.

Check your sending quota
========================

You are limited to only sending a certain amount of messages in a single 24 hour period. Use this code sample to check how many messages you are still allowed to send today. For more information about sending limits see :SES-dg:`Managing Your Amazon SES Sending Limits <manage-sending-limits>`.

**Imports**

.. literalinclude::  example_code/ses/Send_Quota.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Send_Quota.php
   :lines: 26-
   :language: php

Monitor your Sending Activity
=============================

Retrieve metrics for messages you've sent in the past two weeks. This sample will return the number of delivery attempts, bounces, complaints, and rejected messages in 15-minute increments. 

**Imports**

.. literalinclude::  example_code/ses/List_Filters.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Filters.php
   :lines: 26-
   :language: php