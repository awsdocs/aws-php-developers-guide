.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


################################################################
Sending and Receiving Messages in |SQS| with |sdk-php| Version 3 
################################################################

.. meta::
   :description: Deliver, delete, or retrieve messages using Amazon SQS with the AWS SDK for PHP version 3.
   :keywords: Amazon SQS code examples for PHP

To learn about |SQS| messages, see :SQS-dg:`Sending a Message to an SQS Queue <sqs-send-message>`
and :SQS-dg:`Receiving and Deleting a Message from an SQS Queue <sqs-receive-delete-message.html>`
in the |SQS-dg|.

The following examples show how to:


* Deliver a message to a specified queue using :aws-php-class:`SendMessage <api-sqs-2012-11-05.html#sendmessage>`.
* Retrieve one or more messages (up to 10) from a specified queue using :aws-php-class:`ReceiveMessage <api-sqs-2012-11-05.html#receivemessage>`.
* Delete a message from a queue using :aws-php-class:`DeleteMessage <api-sqs-2012-11-05.html#deletemessage>`.

.. include:: text/git-php-examples.txt


Send a Message
==============

**Imports**

.. literalinclude::  example_code/sqs/SendMessage.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sqs/SendMessage.php
   :lines: 31-63
   :language: php

Receive and Delete Messages
===========================

**Imports**

.. literalinclude::  example_code/sqs/ReceiveMessage.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/sqs/ReceiveMessage.php
   :lines: 31-59
   :language: php
