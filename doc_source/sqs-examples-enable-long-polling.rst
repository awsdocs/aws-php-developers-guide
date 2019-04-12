.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


#######################################################
Enabling Long Polling in |SQS| with |sdk-php| Version 3 
#######################################################

.. meta::
   :description: Enable long polling, retrieve messages with long pulling, and create a long polling queue using the AWS SDK for PHP version 3.
   :keywords: Amazon SQS code examples for PHP

Long polling reduces the number of empty responses by allowing |SQS| to wait a specified time for a message to become available in the queue before
sending a response. Also, long polling eliminates false empty responses by querying all of the servers instead of a sampling of servers. To enable
long polling, specify a non-zero wait time for received messages. To learn more, see :SQS-dg:`SQS Long Polling <sqs-long-polling>`.

The following examples show how to:

* Set attributes on an |SQS| queue to enable long polling, using :aws-php-class:`SetQueueAttributes <api-sqs-2012-11-05.html#setqueueattributes>`.
* Retrieve one or more messages with long polling using :aws-php-class:`ReceiveMessage <api-sqs-2012-11-05.html#receivemessage>`.
* Create a long polling queue using :aws-php-class:`CreateQueue <api-sqs-2012-11-05.html#createqueue>`.

.. include:: text/git-php-examples.txt


Set Attributes on a Queue to Enable Long Polling
================================================

**Imports**

.. literalinclude:: sqs.php.long_polling_set_queue_attributes.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sqs.php.long_polling_set_queue_attributes.main.txt
   :language: PHP

Retrieve Messages with Long Polling
===================================

**Imports**

.. literalinclude:: sqs.php.long_polling_recieve_message.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sqs.php.long_polling_recieve_message.main.txt
   :language: PHP

Create a Queue with Long Polling
================================

**Imports**

.. literalinclude:: sqs.php.long_polling_create_queue.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: sqs.php.long_polling_create_queue.main.txt
   :language: PHP
