.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#########################################################################
Creating Delivery Streams Using the |AKF| API and the |sdk-php| Version 3
#########################################################################

.. meta::
   :description: Kinesis Firehose Delivery Streams code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Firehose for PHP, Delivery Streams for PHP, Data Streams for PHP

An alias is an optional display name for an |AKlong| (|AKlong|) :KMS-dg:`customer master key (CMK)<concepts.html#master_keys>`.

The following examples show how to:

* Create a delivery stream using :aws-php-class:`CreateDeliveryStream <api-firehose-2015-08-04.html#createdeliverystream>`.
* View an alias using :aws-php-class:`ListAliases <api-firehose-2015-08-04.html#listaliases>`.
* Update an alias using :aws-php-class:`UpdateAlias <api-firehose-2015-08-04.html#updatealias>`.
* Delete an alias using :aws-php-class:`DeleteAlias <api-firehose-2015-08-04.html#deletealias>`.

.. include:: text/git-php-examples.txt

For more information about using |AKlong| (|AKlong|), see the |KMS-dg|_.

Create a Delivery Stream using a |AK| Data Stream
=================================================

Establish a Delivery Stream that will put data into a classic Kinesis Data Stream using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/CreateDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/CreateDeliveryStream.php
   :lines: 33-
   :language: php

Create a Delivery Stream using an |S3| Bucket
=================================================

Establish a Delivery Stream that will put data into an |S3| Bucket using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/CreateDeliveryS3Stream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/CreateDeliveryS3Stream.php
   :lines: 33-
   :language: php

Create a Delivery Stream using |ES|
===================================

Establish a Delivery Stream that will put data into an |ES| using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/CreateDeliveryESStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/CreateDeliveryESStream.php
   :lines: 33-
   :language: php
   
   
Retrieve a Delivery Stream 
==========================

Get the details about an existing Delivery Stream using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/DescribeDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/DescribeDeliveryStream.php
   :lines: 33-
   :language: php
   
List existing Delivery Streams connected to Kinesis
===================================================

List all the existing Delivery Streams sending data to Kinesis Data Streams using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/ListKinesisDeliveryStreams.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/ListKinesisDeliveryStreams.php
   :lines: 33-
   :language: php

List existing Delivery Streams sending to other |AWS| Services
=====================================================================

List all the existing Delivery Streams sending data to |S3|, |ES|, |RS| or |LAM| using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/ListDirectDeliveryStreams.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/ListDirectDeliveryStreams.php
   :lines: 33-
   :language: php
   
   
Send Data to an existing Delivery Streams 
=========================================

Once you've created a Delivery Stream, use the following example to send data through Firehose Delivery Streams to your specified destination.  

**Imports**

.. literalinclude::  example_code/firehose/PutRecordtoDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/PutRecordtoDeliveryStream.php
   :lines: 33-
   :language: php

Delete a Delivery Stream
========================

Delete a Delivery Stream using the following code sample. 


**Imports**

.. literalinclude::  example_code/firehose/DeleteDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/firehose/DeleteDeliveryStream.php
   :lines: 33-
   :language: php
