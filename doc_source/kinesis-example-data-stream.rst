.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#########################################################################
Creating Data Streams Using the |AKS| API and the |sdk-php| Version 3
#########################################################################

.. meta::
   :description: Kinesis Data Streams code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon Kinesis for PHP, Data Streams for PHP

|AKSlong| allows you send real-time data. Create a data producer with Data Stream, 
which will deliver data to the configured destination every time you add data. Read more about  :AKS-dg:`Creating and Managing Streams 
<working-with-streams.html>` in the |AKS-dg|_.

The following examples show how to:

* Create an alias using :aws-php-class:`CreateAlias <api-kinesis-2013-12-02.html#createstream>`.
* Get details about a single data stream using :aws-php-class:`DescribeStream <api-kinesis-2013-12-02.html#describestream>`.
* List your data streams using :aws-php-class:`ListStreams <api-kinesis-2013-12-02.html#liststreams>`.
* Send Data to a data stream using :aws-php-class:`PutRecord <api-kinesis-2013-12-02.html#putrecord>`.
* Delete a data stream using :aws-php-class:`DeleteStream <api-kinesis-2013-12-02.html#deletestream>`.

.. include:: text/git-php-examples.txt

For more information about using |AKSlong|, see the |AKS-dg|_.

Create a Data Stream using a |AKS| Data Stream
=================================================

Establish a Data Stream that will put data into a classic Kinesis Data Stream using the following code sample. Learn more about :AKS-dg:`Creating and Updating Data Streams <amazon-kinesis-streams.html>`_ in the |AKS-dg|_.


**Imports**

.. literalinclude::  example_code/kinesis/CreateDataStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/CreateDataStream.php
   :lines: 33-
   :language: php

Retrieve a Data Stream 
==========================

Get the details about an existing Data Stream using the following code sample. 


**Imports**

.. literalinclude::  example_code/kinesis/DescribeDataStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/DescribeDataStream.php
   :lines: 33-
   :language: php
   
List existing Data Streams connected to Kinesis
===================================================

List all the existing Data Streams using the following code sample. 


**Imports**

.. literalinclude::  example_code/kinesis/ListDataStreams.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/ListDataStreams.php
   :lines: 33-
   :language: php

   
Send Data to an existing Data Streams 
=========================================

Once you've created a Data Stream, use the following example to send data. 
After you create a Data Stream, use DescribeStream to check the Data StreamStatus is active before sending data to it.  

**Imports**

.. literalinclude::  example_code/kinesis/PutDataStreamRecord.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/PutDataStreamRecord.php
   :lines: 33-
   :language: php

Delete a Data Stream
========================

Delete a Data Stream using the following code sample. This will also delete any data you have sent to the Data Stream.


**Imports**

.. literalinclude::  example_code/kinesis/DeleteDataStream.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/kinesis/DeleteDataStream.php
   :lines: 33-
   :language: php
