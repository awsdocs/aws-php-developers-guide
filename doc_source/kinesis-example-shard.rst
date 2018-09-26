.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################################
Manage Data Shards Streams Using the |AKL| API and the |sdk-php| Version 3
##########################################################################

.. meta::
   :description: Kinesis Data Streams code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Kinesis for PHP, Data Shards for PHP

|AKLlong| allows you send real-time data. Create a data producer with Data Stream, 
which will deliver data to the configured destination every time you add data. 

The following examples show how to:

* List shards in a stream using :aws-php-class:`ListShards <api-kinesis-2013-12-02.html#listshards>`.
* Add or reduce the number of shards in a stream using :aws-php-class:`UpdateShardCount <api-kinesis-2013-12-02.html#updateshardcount>`.

.. include:: text/git-php-examples.txt

For more information about using |AKLlong|, see the |AKL-dg|_.

List Data Stream Shards
=======================

List up to 100 shards in a specific stream. 

**Imports**

.. literalinclude::  example_code/kms/ListDataStreamShards.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/ListDataStreamShards.php
   :lines: 33-
   :language: php

Add more Data Stream Shards
===========================

If you need more Data Stream Shards, you can double your current amount by splitting or updating the number of shards. 

**Imports**

.. literalinclude::  example_code/kms/UpdateDataStreamShards.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kms/UpdateDataStreamShards.php
   :lines: 33-
   :language: php

