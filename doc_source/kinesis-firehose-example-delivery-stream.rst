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
   :description: Kinesis Data Firehose delivery streams code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Kinesis Data Firehose for PHP, delivery streams for PHP, data streams for PHP

|AKFlong| enables you to send real-time data to other AWS services including |AKS|, |S3|, |ESlong| (|ES|), and |RS|, or to Splunk. Create a data producer with delivery streams,
which will deliver data to the configured destination every time you add data.

The following examples show how to:

* Create a delivery stream using :aws-php-class:`CreateDeliveryStream <api-firehose-2015-08-04.html#createdeliverystream>`.
* Get details about a single delivery stream using :aws-php-class:`DescribeDeliveryStream <api-firehose-2015-08-04.html#describedeliverystream>`.
* List your delivery streams using :aws-php-class:`ListDeliveryStreams <api-firehose-2015-08-04.html#listdeliverystreams>`.
* Send data to a delivery stream using :aws-php-class:`PutRecord <api-firehose-2015-08-04.html#putrecord>`.
* Delete a delivery stream using :aws-php-class:`DeleteDeliveryStream <api-firehose-2015-08-04.html#deletedeliverystream>`.

.. include:: text/git-php-examples.txt

For more information about using |AKFlong|, see the |AKF-dg|_.

Create a Delivery Stream Using a |AK| Data Stream
=================================================

Establish a delivery stream that puts data into an existing |AK| data stream using the following code example.

To create a delivery an email address to your |SES| account, use the :AKF-api:`CreateDeliveryStream <API_CreateDeliveryStream>` operation.

**Imports**

.. literalinclude::  example_code/kinesisfirehose/CreateDeliveryStream.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/CreateDeliveryStream.php
   :lines: 32-
   :language: php

Create a Delivery Stream Using an |S3| Bucket
=============================================

Establish a delivery stream that puts data into an existing |S3| bucket using the following code example. Provide the destination parameters,
as described in :AKF-dg:`Destination Parameters <create-destination.html>`. Then ensure that you grant |AKF| access to your |S3| bucket,
as described in :AKF-dg:`Grant Kinesis Data Firehose Access to an Amazon S3 Destination <controlling-access.html#using-iam-s3>`.

**Imports**

.. literalinclude::  example_code/kinesisfirehose/CreateS3DeliveryStream.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/CreateS3DeliveryStream.php
   :lines: 32-
   :language: php

Create a Delivery Stream Using |ES|
===================================

Establish a |AKF| Delivery Stream that will put data into an |ES| using the following code example. Provide the destination parameters, as described in
:AKF-dg:`Destination Parameters <create-destination.html>`. Ensure that you grant |AKF| access to your |ES| cluster, as described in
:AKF-dg:`Grant Kinesis Data Firehose Access to an Amazon ES Destination <controlling-access.html#using-iam-es>`.

**Imports**

.. literalinclude::  example_code/kinesisfirehose/CreateESDeliveryStream.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/CreateESDeliveryStream.php
   :lines: 32-
   :language: php


Retrieve a Delivery Stream
==========================

Get the details about an existing |AKF| Delivery Stream using the following code example.


**Imports**

.. literalinclude::  example_code/kinesisfirehose/DescribeDeliveryStream.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/DescribeDeliveryStream.php
   :lines: 32-
   :language: php

List Existing Delivery Streams Connected to |AKS|
=================================================

List all the existing |AKF| Delivery Streams sending data to |AKS| using the following code example.


**Imports**

.. literalinclude::  example_code/kinesisfirehose/ListKinesisDeliveryStreams.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/ListKinesisDeliveryStreams.php
   :lines: 32-
   :language: php

List Existing Delivery Streams Sending Data to Other |AWS| Services
===================================================================

List all the existing |AKF| Delivery Streams sending data to |S3|, |ES|, or |RS|, or Splunk using the following code example.


**Imports**

.. literalinclude::  example_code/kinesisfirehose/ListDirectDeliveryStreams.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/ListDirectDeliveryStreams.php
   :lines: 32-
   :language: php


Send Data to an Existing |AKF| Delivery Stream
==============================================

Once you've created a |AKF| Delivery Stream, use the following code example to send data through a |AKF| Delivery Stream to your specified destination.
Before sending data to it, use ``DescribeDeliveryStream`` to see if the delivery stream is active.

**Imports**

.. literalinclude::  example_code/kinesisfirehose/PutRecordtoDeliveryStream.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/PutRecordtoDeliveryStream.php
   :lines: 32-
   :language: php

Delete a |AKF| Delivery Stream
==============================

Delete a |AKF| Delivery Stream using the following code example. This also deletes any data you have sent to the delivery stream.


**Imports**

.. literalinclude::  example_code/kinesisfirehose/DeleteDeliveryStream.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesisfirehose/DeleteDeliveryStream.php
   :lines: 32-
   :language: php
