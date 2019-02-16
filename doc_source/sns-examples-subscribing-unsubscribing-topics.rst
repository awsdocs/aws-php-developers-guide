.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

########################################################
Managing Subscriptions in |SNS| with |sdk-php| Version 3
########################################################

.. meta::
   :description: List all subscriptions, subscribe to an email address, an application endpoint, or an AWS Lambda function, or unsubscribe from Amazon SNS topics using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS code examples for PHP


Use |SNSlong| (|SNS|) topics to send notifications to |SQSlong| (|SQS|), HTTP/HTTPS, email addresses, |SMSlong| (|SMS|), or |LAMlong|.

Subscriptions are attached to a topic that manages sending messages to subscribers. Learn more about creating topics in :doc:`sns-examples-managing-topics`.


The following examples show how to:

* Subscribe to an existing topic using :aws-php-class:`Subscribe <api-sns-2010-03-31.html#subscribe>`.
* Verify a subscription using :aws-php-class:`ConfirmSubscription <api-sns-2010-03-31.html#confirmsubscription>`.
* List existing subscriptions using :aws-php-class:`ListSubscriptionsByTopic <api-sns-2010-03-31.html#listsubscriptionsbytopic>`.
* Delete a subscription using :aws-php-class:`Unsubscribe <api-sns-2010-03-31.html#unsubscribe>`.
* Send a message to all subscribers of a topic using :aws-php-class:`Publish <api-sns-2010-03-31.html#publish>`.

For more information about using |SNS|, see :SNS-dg:`Using Amazon SNS for System-to-System Messaging <sns-system-to-system-messaging>`.

.. include:: text/git-php-examples.txt



Subscribe an Email Address to a Topic
=====================================

To initiate a subscription to an email address, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed. This is shown in other examples in this topic.

In this example, the endpoint is an email address. A confirmation token will be sent to this email. Verify the subscription with this confirmation token within three days of receipt.

**Imports**

.. literalinclude:: sns.php.subscribe_email.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.subscribe_email.main.txt
   :language: PHP

Subscribe an Application Endpoint to a Topic
============================================

To initiate a subscription to a web app, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed. This is shown in other examples in this topic.

In this example, the endpoint is a URL. A confirmation token will be sent to this web address. Verify the subscription with this confirmation token within three days of receipt.


**Imports**

.. literalinclude:: sns.php.subscribe_HTTPS.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.subscribe_HTTPS.main.txt
   :language: PHP

Subscribe a |LAM| Function to a Topic
=====================================

To initiate a subscription to a |LAM| function, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed. This is shown in other examples in this topic.

In this example, the endpoint is a |LAM| function. A confirmation token will be sent to this |LAM| function. Verify the subscription with this confirmation token within three days of receipt.


**Imports**

.. literalinclude:: sns.php.subscribe_lambda.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.subscribe_lambda.main.txt
   :language: PHP

Subscribe a Text SMS to a Topic
===============================

To send SMS messages to multiple phone numbers at the same time, subscribe each number to a topic.

To initiate a subscription to a phone number, use the :SNS-api:`Subscribe <API_Subscribe>` operation.

You can use the subscribe method to subscribe several different endpoints to an |SNS| topic, depending on the values used for parameters passed. This is shown in other examples in this topic.

In this example, the endpoint is a phone number in E.164 format, a standard for international telecommunications.

A confirmation token will be sent to this phone number. Verify the subscription with this confirmation token within three days of receipt.

For an alternative way to send SMS messages with |SNS|, see :doc:`sns-examples-sending-sms`.

**Imports**

.. literalinclude:: sns.php.subscribe_text_sms.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.subscribe_text_sms.main.txt
   :language: PHP
   
Confirm Subscription to a Topic
===============================

To actually create a subscription, the endpoint owner must acknowledge intent to receive messages from the topic using a token sent when a subscription is
established initially, as described earlier. Confirmation tokens are valid for three days. After three days, you can resend a token by creating a new subscription.

To confirm a subscription, use the :SNS-api:`ConfirmSubscription <API_ConfirmSubscription>` operation.

**Imports**

.. literalinclude:: sns.php.confirm_subscription.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.confirm_subscription.main.txt
   :language: PHP


List Subscriptions to a Topic
=============================

To list up to 100 existing subscriptions in a given AWS Region, use the :SNS-api:`ListSubscriptions <API_ListSubscriptions>` operation.

**Imports**

.. literalinclude:: sns.php.list_subscriptions.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.list_subscriptions.main.txt
   :language: PHP

Unsubscribe from a Topic
========================

To remove an endpoint subscribed to a topic, use the :SNS-api:`Unsubscribe <API_Unsubscribe>` operation.

If the subscription requires authentication for deletion, only the owner of the subscription or the topic's owner can unsubscribe, and an AWS signature is required.
If the unsubscribe call doesn't require authentication and the requester isn't the subscription owner, a final cancellation message is delivered to the endpoint.


**Imports**

.. literalinclude:: sns.php.unsubscribe.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.unsubscribe.main.txt
   :language: PHP


Publish a Message to an |SNS| Topic
===================================

To deliver a message to each endpoint that's subscribed to an |SNS| topic, use the :SNS-api:`Publish <API_Publish>` operation.

Create an object that contains the parameters for publishing a message, including the message text and the Amazon Resource Name (ARN) of the |SNS| topic.


**Imports**

.. literalinclude:: sns.php.publish_topic.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sns.php.publish_topic.main.txt
   :language: PHP


