.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################################
Manage Data Shards Using the |AKS| API and the |sdk-php| Version 3
##########################################################################

.. meta::
   :description: Kinesis Data Streams code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Kinesis for PHP, Data Shards for PHP

|AKSlong| enables you to send real-time data to an endpoint. The rate of data flow depends on the number of shards in your stream.

You can write 1,000 records per second to a single shard. Each shard also has an upload limit of 1 MiB per second. Usage is calculated and charged on a per-shard basis, so use these examples to manage the data capacity and cost of your stream.

The following examples show how to:

* List shards in a stream using :aws-php-class:`ListShards <api-kinesis-1913-12-02.html#listshards>`.
* Add or reduce the number of shards in a stream using :aws-php-class:`UpdateShardCount <api-kinesis-1913-12-02.html#updateshardcount>`.

.. include:: text/git-php-examples.txt

For more information about using |AKSlong|, see the |AKS-dg|_.

List Data Stream Shards
=======================

List the details of up to 100 shards in a specific stream.

To list the shards in a |AK| data stream, use the :AKS-api:`ListShards <API_ListShards>` operation.

**Imports**

.. literalinclude::  example_code/kinesis/ListDataStreamShards.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/ListDataStreamShards.php
   :lines: 32-
   :language: php

Add More Data Stream Shards
===========================

If you need more data stream shards, you can increase your current number of shards. We recommend that you double your shard count when increasing. This will make a copy of each shard currently available to increase your capacity. You can double the number of your shards only twice in one 24-hour period.

Remember billing for |AKS| usage is calculated per shard, so when demand decreases, we recommend that you reduce your shard count by half. When you remove shards, you can only scale down the amount of shards to half of your current shard count.

To update the shard count of a |AK| data stream, use the :AKS-api:`UpdateShardCount <API_UpdateShardCount>` operation.

**Imports**

.. literalinclude::  example_code/kinesis/UpdateDataStreamShards.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/UpdateDataStreamShards.php
   :lines: 32-
   :language: php
