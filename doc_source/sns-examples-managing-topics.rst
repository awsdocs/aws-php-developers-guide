.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################
Managing Topics in |SQS| with |sdk-php| Version 3 
##########################################################

.. meta::
   :description: How to  create, list, and delete Amazon SNS topics, and to handle topic attributes using the AWS SDK for PHP version 3.
   :keywords: Amazon SNS topics for PHP

A dead letter queue is one that other (source) queues can target for 
messages that can't be processed successfully. You can set aside and 
isolate these messages in the dead letter queue to determine why their 
processing did not succeed. You must individually configure each source 
queue that sends messages to a dead letter queue. Multiple queues can
 target a single dead letter queue.

To learn more, see :SQS-dg:`Using SQS Dead Letter Queues <sqs-dead-letter-queues>`.

The following example shows how to:

* Creates a topic to which notifications can be published using :aws-php-class:`CreateTopic <api-sns-2010-03-31.html#createtopic>`.
* Returns a list of the requester's topics using :aws-php-class:`ListTopics <api-sns-2010-03-31.html#listtopic>`.
* Deletes a topic and all its subscriptions using :aws-php-class:`DeleteTopic <api-sns-2010-03-31.html#deletetopic>`.
* Returns all of the properties of a topic using :aws-php-class:`GetTopicAttributes <api-sns-2010-03-31.html#gettopicattributes>`.
* Allows a topic owner to set an attribute of the topic to a new value using :aws-php-class:`SetTopicAttributes <api-sns-2010-03-31.html#settopicattributes>`.

.. include:: text/git-php-examples.txt


Creating a Topic
==========================

Pass the Name for the new topic to the createTopic method of the AWS.SNS client class. To call the createTopic method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback. The data returned by the promise contains the ARN of the topic.

**Imports**

.. literalinclude::  example_code/sns/CreateTopic.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/CreateTopic.php
   :lines: 31-50
   :language: php

Listing Your Topics
==========================

Create an empty object to pass to the listTopics method of the AWS.SNS client class. To call the listTopics method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback. The data returned by the promise contains an array of your topic ARNs.

**Imports**

.. literalinclude::  example_code/sns/ListTopics.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/ListTopics.php
   :lines: 31-50
   :language: php

Deleting a Topic
==========================

Create an object containing the TopicArn of the topic to delete to pass to the deleteTopic method of the AWS.SNS client class. To call the deleteTopic method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/DeleteTopic.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/DeleteTopic.php
   :lines: 31-50
   :language: php
   
Getting Topic Attributes
==========================

Create an object containing the TopicArn of a topic to delete to pass to the getTopicAttributes method of the AWS.SNS client class. To call the getTopicAttributes method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/GetTopicAttributes.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/GetTopicAttributes.php
   :lines: 31-50
   :language: php
   
Setting Topic Attributes
==========================

Create an object containing the parameters for the attribute update, including the TopicArn of the topic whose attributes you want to set, the name of the attribute to set, and the new value for that attribute. You can set only the Policy, DisplayName, and DeliveryPolicy attributes. Pass the parameters to the setTopicAttributes method of the AWS.SNS client class. To call the setTopicAttributes method, create a promise for invoking an Amazon SNS service object, passing the parameters object. Then handle the response in the promise callback.

**Imports**

.. literalinclude::  example_code/sns/SetTopicAttributes.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sns/SetTopicAttributes.php
   :lines: 31-50
   :language: php