.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

################################################################################
Monitoring Your Sending Activity Using the |SES| API and the |sdk-php| Version 3
################################################################################

.. meta::
   :description: Use the Amazon SES API to monitor your sending reputation.
   :keywords: Amazon SES code examples for PHP, Simple Email Service Quota with PHP, Simple Email Statistics with PHP

|SESlong| (|SES|) provides methods for monitoring your sending activity. We recommend that you implement these methods so that you can keep track of important measures, such as your account's bounce, complaint, and reject rates. Excessively high bounce and complaint rates can jeopardize your ability to send emails using |SES|.

The following examples show how to:

* Check your sending quota using :aws-php-class:`GetSendQuota <api-email-2010-12-01.html#getsendquota>`.
* Monitor your sending activity using :aws-php-class:`GetSendStatistics <api-email-2010-12-01.html#getsendstatistics>`.

.. include:: text/git-php-examples.txt

For more information about using |SES|, see the |SES-dg|_.

Check Your Sending Quota
========================

Your account is limited in the number of emails it can send each second, as well as in the number of emails it can send in a 24-hour period. Use this code to determine both of these limits, and to see how many emails you’ve sent in the last 24 hours. To check how many messages you are still allowed to send, use the :SES-api:`GetSendQuota <API_GetSendQuota>` operation. For more information, see :SES-dg:`Managing Your Amazon SES Sending Limits <manage-sending-limits>`. 

**Imports**

.. literalinclude::  example_code/ses/Send_Quota.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Send_Quota.php
   :lines: 26-
   :language: php

Monitor Your Sending Activity
=============================

To retrieve metrics for messages you've sent in the past two weeks, use the :SES-api:`GetSendStatistics <API_GetSendStatistics>` operation. This example returns the number of delivery attempts, bounces, complaints, and rejected messages in 15-minute increments.

**Imports**

.. literalinclude::  example_code/ses/Send_Statistics.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Send_Statistics.php
   :lines: 26-
   :language: php
