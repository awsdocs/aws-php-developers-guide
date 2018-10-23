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

|AKFlong| allows you send real-time data to other AWS services including |AKS|, |S3|, |ES|, |RS| or Splunk. Create a data producer with Delivery Streams, 
which will deliver data to the configured destination every time you add data. 

The following examples show how to:

* Create a delivery stream using :aws-php-class:`CreateDeliveryStream <api-firehose-2015-08-04.html#createdeliverystream>`.
* Get details about a single delivery stream using :aws-php-class:`DescribeDeliveryStream <api-firehose-2015-08-04.html#describedeliverystream>`.
* List your delivery streams using :aws-php-class:`ListDeliveryStreams <api-firehose-2015-08-04.html#listdeliverystreams>`.
* Send Data to a delivery stream using :aws-php-class:`PutRecord <api-firehose-2015-08-04.html#putrecord>`.
* Delete a delivery stream using :aws-php-class:`DeleteDeliveryStream <api-firehose-2015-08-04.html#deletedeliverystream>`.

.. include:: text/git-php-examples.txt

For more information about using |AKFlong|, see the |AKF-dg|_.

Create a Delivery Stream using a |AK| Data Stream
=================================================

Establish a Delivery Stream that will put data into a classic Kinesis Data Stream using the following code sample. 


**Imports**

.. literalinclude::  example_code/kinesisfirehose/CreateDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/CreateDeliveryStream.php
   :lines: 33-
   :language: php

Create a Delivery Stream using an |S3| Bucket
=================================================

Establish a Delivery Stream that will put data into an existing |S3| Bucket using the following code sample. Provide the destination parameters as described in the :AKF-dg:`Destination Parameters <create-destination.html>` and ensure that you grant |AKF| access to your |S3| Bucket as described in :AKF-dg:`Grant Kinesis Data Firehose Access to an Amazon S3 Destination <controlling-access.html#using-iam-s3>`.


**Imports**

.. literalinclude::  example_code/kinesisfirehose/CreateS3DeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/CreateS3DeliveryStream.php
   :lines: 33-
   :language: php

Create a Delivery Stream using |ES|
===================================

Establish a Delivery Stream that will put data into an |ES| using the following code sample. Provide the destination parameters as described in the :AKF-dg:`Destination Parameters <create-destination.html>` and ensure that you grant |AKF| access to your |ES| cluster as described in :AKF-dg:`Grant Kinesis Data Firehose Access to an Amazon ES Destination <controlling-access.html#using-iam-es>`.

**Imports**

.. literalinclude::  example_code/kinesisfirehose/CreateESDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/CreateESDeliveryStream.php
   :lines: 33-
   :language: php
   
   
Retrieve a Delivery Stream 
==========================

Get the details about an existing Delivery Stream using the following code sample. 


**Imports**

.. literalinclude::  example_code/kinesisfirehose/DescribeDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/DescribeDeliveryStream.php
   :lines: 33-
   :language: php
   
List existing Delivery Streams connected to Kinesis
===================================================

List all the existing Delivery Streams sending data to Kinesis Data Streams using the following code sample. 


**Imports**

.. literalinclude::  example_code/kinesisfirehose/ListKinesisDeliveryStreams.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/ListKinesisDeliveryStreams.php
   :lines: 33-
   :language: php

List existing Delivery Streams sending to other |AWS| Services
=====================================================================

List all the existing Delivery Streams sending data to |S3|, |ES|, |RS| or Splunk using the following code sample. 


**Imports**

.. literalinclude::  example_code/kinesisfirehose/ListDirectDeliveryStreams.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/ListDirectDeliveryStreams.php
   :lines: 33-
   :language: php
   
   
Send Data to an existing Delivery Streams 
=========================================

Once you've created a Delivery Stream, use the following example to send data through Firehose Delivery Streams to your specified destination. 
After you create a Delivery Stream, use DescribeDeliveryStream to see if the Delivery Stream is active before sending data to it.  

**Imports**

.. literalinclude::  example_code/kinesisfirehose/PutRecordtoDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/PutRecordtoDeliveryStream.php
   :lines: 33-
   :language: php

Delete a Delivery Stream
========================

Delete a Delivery Stream using the following code sample. This will also delete any data you have sent to the Delivery Stream.


**Imports**

.. literalinclude::  example_code/kinesisfirehose/DeleteDeliveryStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/DeleteDeliveryStream.php
   :lines: 33-
   :language: php
