.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Sending SMS Messages in |SNS| with |sdk-php| Version 3 
##########################################################

.. meta::
   :description: Get and set SMS messaging preferences, check a phone number,  and get a list of phone numbers for Amazon SNS using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS code examples for PHP, Amazon SNS for PHP, Send SMS messages with Simple Notification Service

You can use |SNS| to send text messages, or SMS messages,  to SMS-enabled devices. You can send a message directly to a phone number, or you can send a message to multiple phone numbers at once by subscribing those phone numbers to a topic and sending your message to the topic.

Use |SNS| to specify preferences for SMS messaging, such as how your deliveries are optimized (for cost or for reliable delivery), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports. These preferences are retrieved and set as SMS attributes for |SNS|.

When you send an SMS message, specify the phone number using the E.164 format. E.164 is a standard for the phone number structure used for international telecommunication. Phone numbers that follow this format can have a maximum of 15 digits, and they are prefixed with the plus character (+) and the country code. For example, a US phone number in E.164 format would appear as +1001XXX5550100.


The following example shows how to:

* Retrieve the default settings for sending SMS messages from your account using :aws-php-class:`GetSMSAttributes <api-sns-2010-03-31.html#getsmsattributes>`.
* Update the default settings for sending SMS messages from your account using :aws-php-class:`SetSMSAttributes <api-sns-2010-03-31.html#setsmsattributes>`.
* Discover if a given phone number owner has opted out of receiving SMS messages from your account using :aws-php-class:`CheckIfPhoneNumberISOptedOut <api-sns-2010-03-31.html#checkifphonenumberisoptedout>`.
* List phone numbers where the owner has opted out of receiving SMS messages from your account  using :aws-php-class:`ListPhoneNumberOptedOut <api/api-sns-2010-03-31.html#listphonenumbersoptedout>`.
* Send a text message (SMS message) directly to a phone number using :aws-php-class:`Publish <api-sns-2010-03-31.html#publish>`.

For more information about using |SNS|, see :SNS-dg:`Using Amazon SNS for User Notifications with a Mobile Phone Number as a Subscriber (Send SMS) <sns-mobile-phone-number-as-subscriber>`.

.. include:: text/git-php-examples.txt


Getting SMS Attributes
======================

To retrieve the default settings for SMS messages, use the :SNS-api:`GetSMSAttributes <API_GetSMSAttributes>` operation.

This example gets the DefaultSMSType attribute, which controls whether SMS messages are sent as Promotional, which optimizes message delivery to incur the lowest cost, or as Transactional, which optimizes message delivery to achieve the highest reliability. 

**Imports**

.. literalinclude::  example_code/sns/GetSMSAtrributes.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/GetSMSAtrributes.php
   :lines: 31-50
   :language: php
   
Setting SMS Attributes
======================

To update the default settings for SMS messages, use the :SNS-api:`SetSMSAttributes <API_SetSMSAttributes>` operation.

This example sets the DefaultSMSType attribute to Transactional, which optimizes message delivery to achieve the highest reliability. 

**Imports**

.. literalinclude::  example_code/sns/SetSMSAtrributes.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SetSMSAtrributes.php
   :lines: 31-50
   :language: php
   
Checking If a Phone Number Has Opted Out
========================================

To determine if a given phone number owner has opted out of receiving SMS messages from your account, use the :SNS-api:`CheckIfPhoneNumberIsOptedOut <API_CheckIfPhoneNumberIsOptedOut>` operation.

In this example the phone number is E.164 format, a standard for international telecommunication.

**Imports**

.. literalinclude::  example_code/sns/CheckOptOut.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/CheckOptOut.php
   :lines: 31-50
   :language: php
   
   
Listing Opted-Out Phone Numbers
========================================

To retrieve a list of phone numbers where owner has opted out of receiving SMS messages from your account, use the :SNS-api:`ListPhoneNumbersOptedOut <API_ListPhoneNumbersOptedOut>` operation.

**Imports**

.. literalinclude::  example_code/sns/ListOptOut.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/ListOptOut.php
   :lines: 31-50
   :language: php
   
Publishing to a SMS Text Message 
================================

To deliver a text message (SMS message) directly to a phone number, use the :SNS-api:`Publish <API_Publish>` operation.

In this example the phone number is E.164 format, a standard for international telecommunication. 

SMS message can contain up to 140 bytes. The total size limit for a single SMS publish action is 1600 bytes

For more details on sending SMS messages, see :SNS-dg:`Sending an SMS Message <sms_publish-to-phone>`.

**Imports**

.. literalinclude::  example_code/sns/PublishTextSMS.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/PublishTextSMS.php
   :lines: 31-50
   :language: php