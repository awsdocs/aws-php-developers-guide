.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Publishing Managing in |SQS| with |sdk-php| Version 3 
##########################################################

.. meta::
   :description: Enable dead letter queues with Amazon SQS using the AWS SDK for PHP version 3.
   :keywords: Amazon SQS code examples for PHP

A dead letter queue is one that other (source) queues can target for messages that can't be processed successfully. You can set aside and isolate these messages
in the dead letter queue to determine why their processing did not succeed. You must individually configure each source queue that sends messages to a dead letter
queue. Multiple queues can target a single dead letter queue.

To learn more, see :SQS-dg:`Using SQS Dead Letter Queues <sqs-dead-letter-queues>`.

The following example shows how to:

* Sends a message to an using :aws-php-class:`Publish <api-sns-2010-03-31.html#publish>`.

.. include:: text/git-php-examples.txt


Publishing a Message to an |SNS| Topic
======================================

Create an object containing the parameters for publishing a message, including the message text and the ARN of the SNS topic. For details on available SMS attributes, see SetSMSAttributes in the Amazon Simple Notification Service API Reference of the SNS topic.

Pass the parameters to the publish method of the AWS.SNS client class. Create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/PublishTopic.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/PublishTopic.php
   :lines: 31-50
   :language: php
 