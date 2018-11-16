.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#############################################################
Managing Visibility Timeout in |SQS| with |sdk-php| Version 3 
#############################################################

.. meta::
   :description: Change the visibility timeout for messages in Amazon SQS using the AWS SDK for PHP version 3.
   :keywords: Amazon SQS code examples for PHP

A visibility timeout is a period of time during which |SQS| prevents other consuming components from receiving and processing a message. To learn more, see `Visibility Timeout <http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html>`_.

The following example shows how to:


* Change the visibility timeout of specified messages in a queue to new values, using :aws-php-class:`ChangeMessageVisibilityBatch <api-sqs-2012-11-05.html#changemessagevisibilitybatch>`.

.. include:: text/git-php-examples.txt



Change the Visibility Timeout of Multiple Messages
==================================================

**Imports**

.. literalinclude::  example_code/sqs/ChangeMessageVisibilityBatch.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sqs/ChangeMessageVisibilityBatch.php
   :lines: 32-69
   :language: php
