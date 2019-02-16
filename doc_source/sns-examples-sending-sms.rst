.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Sending SMS Messages in |SNS| with the |sdk-php| Version 3
##########################################################

.. meta::
   :description: Get and set SMS messaging preferences, check a phone number, and get a list of phone numbers for Amazon SNS using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS code examples for PHP, Amazon SNS for PHP, send SMS messages with Simple Notification Service

You can use |SNSlong| (|SNS|) to send text messages, or SMS messages, to SMS-enabled devices. You can send a message directly to a phone number, or you can send a message to multiple phone numbers at once by subscribing those phone numbers to a topic and sending your message to the topic.

Use |SNS| to specify preferences for SMS messaging, such as how your deliveries are optimized (for cost or for reliable delivery), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports. These preferences are retrieved and set as SMS attributes for |SNS|.

When you send an SMS message, specify the phone number using the E.164 format. E.164 is a standard for the phone number structure used for international telecommunications. Phone numbers that follow this format can have a maximum of 15 digits, and are prefixed with the plus character (+) and the country code. For example, a US phone number in E.164 format would appear as +1001XXX5550100.

The following examples show how to:

* Retrieve the default settings for sending SMS messages from your account using :aws-php-class:`GetSMSAttributes <api-sns-2010-03-31.html#getsmsattributes>`.
* Update the default settings for sending SMS messages from your account using :aws-php-class:`SetSMSAttributes <api-sns-2010-03-31.html#setsmsattributes>`.
* Discover if a given phone number owner has opted out of receiving SMS messages from your account using :aws-php-class:`CheckIfPhoneNumberISOptedOut <api-sns-2010-03-31.html#checkifphonenumberisoptedout>`.
* List phone numbers where the owner has opted out of receiving SMS messages from your account  using :aws-php-class:`ListPhoneNumberOptedOut <api/api-sns-2010-03-31.html#listphonenumbersoptedout>`.
* Send a text message (SMS message) directly to a phone number using :aws-php-class:`Publish <api-sns-2010-03-31.html#publish>`.

For more information about using |SNS|, see :SNS-dg:`Using Amazon SNS for User Notifications with a Mobile Phone Number as a Subscriber (Send SMS) <sns-mobile-phone-number-as-subscriber>`.

.. include:: text/git-php-examples.txt


Get SMS Attributes
==================

To retrieve the default settings for SMS messages, use the :SNS-api:`GetSMSAttributes <API_GetSMSAttributes>` operation.

This example gets the :code:`DefaultSMSType` attribute. This attribute controls whether SMS messages are sent as :code:`Promotional`, which optimizes message delivery to incur the lowest cost, or as :code:`Transactional`, which optimizes message delivery to achieve the highest reliability.

**Imports**

.. literalinclude:: sns.php.get_sms_attributes.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.get_sms_attributes.main.txt
   :language: PHP

Set SMS Attributes
==================

To update the default settings for SMS messages, use the :SNS-api:`SetSMSAttributes <API_SetSMSAttributes>` operation.

This example sets the :code:`DefaultSMSType` attribute to :code:`Transactional`, which optimizes message delivery to achieve the highest reliability.

**Imports**

.. literalinclude:: sns.php.set_sms_attributes.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.set_sms_attributes.main.txt
   :language: PHP

Check If a Phone Number Has Opted Out
=====================================

To determine if a given phone number owner has opted out of receiving SMS messages from your account, use the :SNS-api:`CheckIfPhoneNumberIsOptedOut <API_CheckIfPhoneNumberIsOptedOut>` operation.

In this example, the phone number is in E.164 format, a standard for international telecommunications.

**Imports**

.. literalinclude:: sns.php.check_opt_out.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.check_opt_out.main.txt
   :language: PHP


List Opted-Out Phone Numbers
============================

To retrieve a list of phone numbers where the owner has opted out of receiving SMS messages from your account, use the :SNS-api:`ListPhoneNumbersOptedOut <API_ListPhoneNumbersOptedOut>` operation.

**Imports**

.. literalinclude:: sns.php.list_optout.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.list_optout.main.txt
   :language: PHP

Publish to a Text Message (SMS Message)
=======================================

To deliver a text message (SMS message) directly to a phone number, use the :SNS-api:`Publish <API_Publish>` operation.

In this example, the phone number is in E.164 format, a standard for international telecommunications.

SMS messages can contain up to 140 bytes. The size limit for a single SMS publish action is 1,600 bytes.

For more details on sending SMS messages, see :SNS-dg:`Sending an SMS Message <sms_publish-to-phone>`.

**Imports**

.. literalinclude:: sns.php.publish_text_SMS.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.publish_text_SMS.main.txt
   :language: PHP
