.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#####################################################################
Creating Data Streams Using the |AKS| API and the |sdk-php| Version 3
#####################################################################

.. meta::
   :description: Amazon Kinesis Data Streams code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Kinesis for PHP, Amazon Kinesis Data Streams for PHP

|AKSlong| allows you to send real-time data. Create a data producer with |AKS|
that delivers data to the configured destination every time you add data.

For more information, see  :AKS-dg:`Creating and Managing Streams
<working-with-streams.html>` in the |AKS-dg|.

The following examples show how to:

* Create an alias using :aws-php-class:`CreateAlias <api-kinesis-2013-12-02.html#createstream>`.
* Get details about a single data stream using :aws-php-class:`DescribeStream <api-kinesis-2013-12-02.html#describestream>`.
* List your data streams using :aws-php-class:`ListStreams <api-kinesis-2013-12-02.html#liststreams>`.
* Send data to a data stream using :aws-php-class:`PutRecord <api-kinesis-2013-12-02.html#putrecord>`.
* Delete a data stream using :aws-php-class:`DeleteStream <api-kinesis-2013-12-02.html#deletestream>`.

.. include:: text/git-php-examples.txt

For more information about using |AKSlong|, see the |AKS-dg|_.

Create a Data Stream Using a |AK| Data Stream
=============================================

Establish a |AK| data stream where you can send information to be processed by |AK| using the following code example. Learn more about
:AKS-dg:`Creating and Updating Data Streams <amazon-kinesis-streams>` in the |AKS-dg|.

To create a |AK| data stream, use the :AKS-api:`CreateStream <API_CreateStream>` operation.

**Imports**

.. literalinclude::  example_code/kinesis/CreateDataStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/CreateDataStream.php
   :lines: 33-
   :language: php

Retrieve a Data Stream
======================

Get details about an existing data stream using the following code example. By default, this returns information about the first 10 shards connected to the specified |AK| data stream. Remember to check :code:`StreamStatus` from the response before writing data to a |AK| data stream.

To retrieve details about a specified |AK| data stream, use the :AKS-api:`DescribeStream <API_DescribeStream>` operation.

**Imports**

.. literalinclude::  example_code/kinesis/DescribeDataStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/DescribeDataStream.php
   :lines: 33-
   :language: php

List Existing Data Streams That Are Connected to |AK|
=====================================================

List the first 10 data streams from your AWS account in the selected AWS Region. Use the returned ```HasMoreStreams`` to determine if there are more streams associated with your account.

To list your |AK| data streams, use the :AKS-api:`ListStreams <API_ListStreams>` operation.

**Imports**

.. literalinclude::  example_code/kinesis/ListDataStreams.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/ListDataStreams.php
   :lines: 33-
   :language: php


Send Data to an Existing Data Stream
====================================

Once you create a data stream, use the following example to send data.
Before sending data to it, use ``DescribeStream`` to check whether the data :code:`StreamStatus` is active.

To write a single data record to a |AK| data stream, use the :AKS-api:`PutRecord <API_PutRecord>` operation. To write up to 500 records into a |AK| data stream, use the :AKS-api:`PutRecords <API_PutRecords>` operation.

**Imports**

.. literalinclude::  example_code/kinesis/PutDataStreamRecord.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/PutDataStreamRecord.php
   :lines: 33-
   :language: php

Delete a Data Stream
====================

This example demonstrates how to delete a data stream. Deleting a data stream also deletes any data you sent to the data stream. Active |AK| data streams switch to the DELETING state until the stream deletion is complete. While in the DELETING state, the stream continues to process data.

To delete a |AK| data stream, use the :AKS-api:`DeleteStream <API_DeleteStream>` operation.


**Imports**

.. literalinclude::  example_code/kinesis/DeleteDataStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/DeleteDataStream.php
   :lines: 33-
   :language: php
