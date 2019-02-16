.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

|AKFlong| enables you to send real-time data to other AWS services including |AKSlong|, |S3|, |ESlong| (|ES|), and |RS|, or to Splunk. Create a data producer with delivery streams
to deliver data to the configured destination every time you add data.

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

To establish a delivery stream that puts data into an existing |AK| data stream,
use the :AKF-api:`CreateDeliveryStream <API_CreateDeliveryStream>` operation.

This enables developers to migrate existing |AK| services to |AKF|.

**Imports**

.. literalinclude:: firehose.php.create_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.create_delivery_stream.main.txt
   :language: PHP

Create a Delivery Stream Using an |S3| Bucket
=============================================

To establish a delivery stream that puts data into an existing |S3| bucket,
use the :AKF-api:`CreateDeliveryStream <API_CreateDeliveryStream>` operation.

Provide the destination parameters,
as described in :AKF-dg:`Destination Parameters <create-destination>`. Then ensure that you grant |AKF| access to your |S3| bucket,
as described in :AKF-dg:`Grant Kinesis Data Firehose Access to an Amazon S3 Destination <controlling-access.html#using-iam-s3>`.

**Imports**

.. literalinclude:: firehose.php.create_s3_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.create_s3_delivery_stream.main.txt
   :language: PHP


Create a Delivery Stream Using |ES|
===================================

To establish a |AKF| delivery stream that puts data into an |ES| cluster,
use the :AKF-api:`CreateDeliveryStream <API_CreateDeliveryStream>` operation.

Provide the destination parameters, as described in
:AKF-dg:`Destination Parameters <create-destination>`. Ensure that you grant |AKF| access to your |ES| cluster, as described in
:AKF-dg:`Grant Kinesis Data Firehose Access to an Amazon ES Destination <controlling-access.html#using-iam-es>`.

**Imports**

.. literalinclude:: firehose.php.create_es_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.create_es_delivery_stream.main.txt
   :language: PHP



Retrieve a Delivery Stream
==========================

To get the details about an existing |AKF| delivery stream,
use the :AKF-api:`DescribeDeliveryStream <API_DescribeDeliveryStream>` operation.

**Imports**

.. literalinclude:: firehose.php.describe_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.describe_delivery_stream.main.txt
   :language: PHP

List Existing Delivery Streams Connected to |AKS|
=================================================

To list all the existing |AKF| delivery streams sending data to |AKS|,
use the :AKF-api:`ListDeliveryStreams <API_ListDeliveryStreams>` operation.

**Imports**

.. literalinclude:: firehose.php.list_kinesis_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.list_kinesis_delivery_stream.main.txt
   :language: PHP

List Existing Delivery Streams Sending Data to Other |AWS| Services
===================================================================

To list all the existing |AKF| delivery streams sending data to |S3|, |ES|, or |RS|, or to Splunk,
use the :AKF-api:`ListDeliveryStreams <API_ListDeliveryStreams>` operation.

**Imports**

.. literalinclude:: firehose.php.list_direct_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.list_direct_delivery_stream.main.txt
   :language: PHP


Send Data to an Existing |AKF| Delivery Stream
==============================================

To send data through a |AKF| delivery stream to your specified destination,
use the :AKF-api:`PutRecord <API_API_PutRecord>` operation after you create a |AKF| delivery stream.

Before sending data to a |AKF| delivery stream, use ``DescribeDeliveryStream`` to see if the delivery stream is active.

**Imports**

.. literalinclude:: firehose.php.put_record_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.put_record_delivery_stream.main.txt
   :language: PHP

Delete a |AKF| Delivery Stream
==============================

To delete a |AKF| delivery stream,
use the :AKF-api:`DeleteDeliveryStreams <API_DeleteDeliveryStreams>` operation.
This also deletes any data you have sent to the delivery stream.

**Imports**

.. literalinclude:: firehose.php.delete_delivery_stream.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: firehose.php.delete_delivery_stream.main.txt
   :language: PHP