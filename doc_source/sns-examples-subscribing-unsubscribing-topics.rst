.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Managing Subscriptions in |SNS| with |sdk-php| Version 3 
##########################################################

.. meta::
   :description: List all subscriptions, subscribe an email address, an application endpoint, or an AWS Lambda function or unsubscribe from Amazon SNS topics using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS code examples for PHP


Use |SNS| topics to send notifications to |SQS|, HTTP/S, email addresses, |SMS|, or |LAM|. 

Subscriptions are attached to a topic that manages sending messages to subscribers. Learn more about creating topics in :doc:`sns-examples-managing-topics`


The following example shows how to:

* Subscribe to an existing topic using :aws-php-class:`Subscribe <api-sns-2010-03-31.html#subscribe>`.
* Verify a subscription using :aws-php-class:`ConfirmSubscription <api-sns-2010-03-31.html#confirmsubscription>`.
* List existing subscriptions using :aws-php-class:`ListSubscriptionsByTopic <api-sns-2010-03-31.html#listsubscriptionsbytopic>`.
* Delete a subscription using :aws-php-class:`Unsubscribe <api-sns-2010-03-31.html#unsubscribe>`.
* Sends a message to all subscribers of a topic using :aws-php-class:`Publish <api-sns-2010-03-31.html#publish>`.

For more information about using |SNS|, see :SNS-dg:`Using Amazon SNS for System-to-System Messaging <sns-system-to-system-messaging>`.

.. include:: text/git-php-examples.txt



Subscribing an Email Address to a Topic
=======================================

To initiate a subscription to an email address, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed, as other examples in this topic will show.

In this example the endpoint is an email address. A confirmation token will be sent to this email. Verify the subscription with this confirmation token within 3 days of receipt. 

**Imports**

.. literalinclude::  example_code/sns/SubscribeEmail.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SubscribeEmail.php
   :lines: 31-50
   :language: php
   
Subscribing an Application Endpoint to a Topic
==============================================

To initiate a subscription to an web app, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed, as other examples in this topic will show.

In this example the endpoint is a url. A confirmation token will be sent to this web address. Verify the subscription with this confirmation token within 3 days of receipt. 


**Imports**

.. literalinclude::  example_code/sns/SubscribeHTTPS.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SubscribeHTTPS.php
   :lines: 31-50
   :language: php
   
Subscribing a Lambda Function to a Topic
==============================================

To initiate a subscription to an |LAM|, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed, as other examples in this topic will show.

In this example the endpoint is an |LAM|. A confirmation token will be sent to this |LAM|. Verify the subscription with this confirmation token within 3 days of receipt. 


**Imports**

.. literalinclude::  example_code/sns/SubscribeLambda.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SubscribeLambda.php
   :lines: 31-50
   :language: php
   
Subscripting a Text SMS to a Topic
=================================

If you want to send SMS messages to multiple numbers at the same time, subscribe each number to a topic. 

To initiate a subscription to a phone number, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed, as other examples in this topic will show.

In this example the endpoint is an phone number in E.164 format, a standard for international telecommunication. 

A confirmation token will be sent to this phone number. Verify the subscription with this confirmation token within 3 days of receipt. 

For an alternative way to send SMS messages with |SNS| see :doc:`sns-examples-sending-sms`.


**Imports**

.. literalinclude::  example_code/sns/SubscribeTextSMS.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SubscribeTextSMS.php
   :lines: 31-50
   :language: php
   
Confirm Subscription to a Topic
===============================

To actually create a subscription, the endpoint owner must acknowledge intent to receive messages from the topic with a token sent when a subscriptions is initially 
established as described above. Confirmation tokens are valid for three days. After three days, you can resend a token by creating a new subscription. 

To confirm a subscription, use the :SNS-api:`ConfirmSubscription <API_ConfirmSubscription>` operation.



**Imports**

.. literalinclude::  example_code/sns/ConfirmSubscription.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/ConfirmSubscription.php
   :lines: 31-50
   :language: php
   

Listing Subscriptions to a Topic
================================

To list up to 100 existing subscriptions in a given AWS region, use the :SNS-api:`ListSubscriptions <API_ListSubscriptions>` operation.

**Imports**

.. literalinclude::  example_code/sns/ListSubscriptions.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/ListSubscriptions.php
   :lines: 31-50
   :language: php
   
Unsubscribing from a Topic
==========================

To remove an endpoint subscribed to a topic, use the :SNS-api:`Unsubscribe <API_Unsubscribe>` operation.



If the subscription requires authentication for deletion, only the owner of the subscription or the topic's owner can unsubscribe, and an AWS signature is required. 
If the Unsubscribe call does not require authentication and the requester is not the subscription owner, a final cancellation message is delivered to the endpoint

To call the unsubscribe method, 

**Imports**

.. literalinclude::  example_code/sns/Unsubscribe.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/Unsubscribe.php
   :lines: 31-50
   :language: php
   

Publishing a Message to an |SNS| Topic
======================================

To deliver a message to each endpoint that is subscribed to an SNS Topic, use the :SNS-api:`Publish <API_Publish>` operation.

Create an object containing the parameters for publishing a message, including the message text and the ARN of the SNS topic. 


**Imports**

.. literalinclude::  example_code/sns/PublishTopic.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/PublishTopic.php
   :lines: 31-50
   :language: php
 