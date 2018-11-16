.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Managing Subscriptions in |SQS| with |sdk-php| Version 3 
##########################################################

.. meta::
   :description: List all subscriptions,subscribe an email address, an application endpoint, or an AWS Lambda function or unsubscribe from Amazon SNS topics using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS code examples for PHP

List all subscriptions,subscribe an email address, an application endpoint, or an AWS Lambda function or unsubscribe from Amazon SNS topics.

The following example shows how to:

* Enable a dead letter queue using :aws-php-class:`Subscribe <api-sns-2010-03-31.html#subscribe>`.
* Enable a dead letter queue using :aws-php-class:`ConfirmSubscription <api-sns-2010-03-31.html#confirmsubscription>`.
* Enable a dead letter queue using :aws-php-class:`ListSubscriptionsByTopic <api-sns-2010-03-31.html#listsubscriptionsbytopic>`.
* Enable a dead letter queue using :aws-php-class:`Unsubscribe <api-sns-2010-03-31.html#unsubscribe>`.

.. include:: text/git-php-examples.txt


Listing Subscriptions to a Topic
================================

Configure the SDK as previously shown.

Create an object containing the TopicArn parameter for the topic whose subscriptions you want to list. Pass the parameters to the listSubscriptionsByTopic method of the AWS.SNS client class. To call the listSubscriptionsByTopic method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/ListSubscriptions.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/ListSubscriptions.php
   :lines: 31-50
   :language: php

Subscribing an Email Address to a Topic
=======================================

Create an object containing the Protocol parameter to specify the email protocol, the TopicArn for the topic to subscribe to, and an email address as the message Endpoint. Pass the parameters to the subscribe method of the AWS.SNS client class. You can use the subscribe method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed, as other examples in this topic will show.

To call the subscribe method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

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

Create an object containing the Protocol parameter to specify the application protocol, the TopicArn for the topic to subscribe to, and the ARN of a mobile application endpoint for the Endpoint parameter. Pass the parameters to the subscribe method of the AWS.SNS client class.

To call the subscribe method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

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

Create an object containing the Protocol parameter, specifying the lambda protocol, the TopicArn for the topic to subscribe to, and the ARN of an AWS Lambda function as the Endpoint parameter. Pass the parameters to the subscribe method of the AWS.SNS client class.

To call the subscribe method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/SubscribeLambda.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SubscribeLambda.php
   :lines: 31-50
   :language: php
   
Subscriping a Text SMS to a Topic
=================================

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

**Imports**

.. literalinclude::  example_code/sns/ConfirmSubscription.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/ConfirmSubscription.php
   :lines: 31-50
   :language: php
   

   
Unsubscribing from a Topic
==========================

Create an object containing the SubscriptionArn parameter, specifying the ARN of the subscription to unsubscribe. Pass the parameters to the unsubscribe method of the AWS.SNS client class.

To call the unsubscribe method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/Unsubscribe.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/Unsubscribe.php
   :lines: 31-50
   :language: php
   
   
