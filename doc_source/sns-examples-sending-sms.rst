.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Sending SMS Messages in |SQS| with |sdk-php| Version 3 
##########################################################

.. meta::
   :description: Get and set SMS messaging preferences, check a phone number,  and get a list of phone numbers for Amazon SNS using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS code examples for PHP, Amazon SNS for PHP

You can use Amazon SNS to send text messages, or SMS messages,  to SMS-enabled devices. You can send a message directly to a phone number, or you can send a message to multiple phone numbers at once by subscribing those phone numbers to a topic and sending your message to the topic.

The following example shows how to:

* Returns the settings for sending SMS messages from your account using :aws-php-class:`GetSMSAttributes <api-sns-2010-03-31.html#getsmsattributes>`.
* Returns the settings for sending SMS messages from your account using :aws-php-class:`SetSMSAttributes <api-sns-2010-03-31.html#setsmsattributes>`.
* Returns the settings for sending SMS messages from your account using :aws-php-class:`CheckIfPhoneNumberISOptedOut <api-sns-2010-03-31.html#checkifphonenumberisoptedout>`.
* Returns the settings for sending SMS messages from your account using :aws-php-class:`ListPhoneNumberOptedOut <api/api-sns-2010-03-31.html#listphonenumbersoptedout>`.
* Returns the settings for sending SMS messages from your account using :aws-php-class:`Publish <api-sns-2010-03-31.html#publish>`.

.. include:: text/git-php-examples.txt


Getting SMS Attributes
==========================

Use Amazon SNS to specify preferences for SMS messaging, such as how your deliveries are optimized (for cost or for reliable delivery), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports. These preferences are retrieved and set as SMS attributes for Amazon SNS.

Create an object containing the parameters for getting SMS attributes, including the names of the individual attributes to get. For details on available SMS attributes, see SetSMSAttributes in the Amazon Simple Notification Service API Reference.

This example gets the DefaultSMSType attribute, which controls whether SMS messages are sent as Promotional, which optimizes message delivery to incur the lowest cost, or as Transactional, which optimizes message delivery to achieve the highest reliability. Pass the parameters to the setTopicAttributes method of the AWS.SNS client class. To call the getSMSAttributes method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/
   :lines: 31-50
   :language: php
   
Setting SMS Attributes
==========================

Create an object containing the parameters for setting SMS attributes, including the names of the individual attributes to set and the values to set for each. For details on available SMS attributes, see SetSMSAttributes in the Amazon Simple Notification Service API Reference.

This example sets the DefaultSMSType attribute to Transactional, which optimizes message delivery to achieve the highest reliability. Pass the parameters to the setTopicAttributes method of the AWS.SNS client class. To call the getSMSAttributes method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/
   :lines: 31-50
   :language: php
   
Checking If a Phone Number Has Opted Out
========================================

Create an object containing the phone number to check as a parameter.

This example sets the PhoneNumber parameter to specify the phone number to check. Pass the object to the checkIfPhoneNumberIsOptedOut method of the AWS.SNS client class. To call the checkIfPhoneNumberIsOptedOut method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

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

Create an empty object as a parameter.

Pass the object to the listPhoneNumbersOptedOut method of the AWS.SNS client class. To call the listPhoneNumbersOptedOut method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

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

Create an object containing the Message and PhoneNumber parameters.

When you send an SMS message, specify the phone number using the E.164 format. E.164 is a standard for the phone number structure used for international telecommunication. Phone numbers that follow this format can have a maximum of 15 digits, and they are prefixed with the plus character (+) and the country code. For example, a US phone number in E.164 format would appear as +1001XXX5550100.

This example sets the PhoneNumber parameter to specify the phone number to send the message. Pass the object to the publish method of the AWS.SNS client class. To call the publish method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/PublishTextSMS.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/PublishTextSMS.php
   :lines: 31-50
   :language: php