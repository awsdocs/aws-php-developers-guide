.. Copyright 2010-2018
 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

=============================
Getting Metrics from |CWlong|
=============================

.. meta::
   :description: List Amazon CloudWatch metrics, retrieve alarms for metrics, and get metric statistics using the AWS SDK for PHP.
   :keywords: Amazon CloudWatch code examples for PHP

Metrics are data about the performance of your systems. You can enable detailed monitoring of some resources,
such as your |EC2| instances, or of your own application metrics.

The following examples show how to:

* List metrics using :aws-php-class:`ListMetrics </api-monitoring-2010-08-01.html#listmetrics>`.
* Retrieve alarms for a metric using :aws-php-class:`DescribeAlarmsForMetric </api-monitoring-2010-08-01.html#describealarmsformetric>`.
* Get statistics for a specified metric using :aws-php-class:`GetMetricStatistics </api-monitoring-2010-08-01.html#getmetricstatistics>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials. See :doc:`guide_credentials`.

List Metrics
------------

**Imports**

.. literalinclude::  example_code/cloudwatch/ListMetrics.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/ListMetrics.php
   :lines: 31-43
   :language: php


Retrieve Alarms for a Metric
----------------------------

**Imports**

.. literalinclude::  example_code/cloudwatch/DescribeAlarmsForMetric.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/DescribeAlarmsForMetric.php
   :lines: 31-48
   :language: php

Get Metric Statistics
---------------------

**Imports**

.. literalinclude::  example_code/cloudwatch/GetMetricStatistics.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/GetMetricStatistics.php
   :lines: 31-53
   :language: php