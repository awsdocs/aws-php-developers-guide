Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

######################################################
Getting Metrics from |CWlong| with |sdk-php| Version 3
######################################################

.. meta::
   :description: List Amazon CloudWatch metrics, retrieve alarms for metrics, and get metric statistics using the AWS SDK for PHP version 3.
   :keywords: Amazon CloudWatch code examples for PHP

Metrics are data about the performance of your systems. You can enable detailed monitoring of some resources,
such as your |EC2| instances, or of your own application metrics.

The following examples show how to:

* List metrics using :aws-php-class:`ListMetrics </api-monitoring-2010-08-01.html#listmetrics>`.
* Retrieve alarms for a metric using :aws-php-class:`DescribeAlarmsForMetric </api-monitoring-2010-08-01.html#describealarmsformetric>`.
* Get statistics for a specified metric using :aws-php-class:`GetMetricStatistics </api-monitoring-2010-08-01.html#getmetricstatistics>`.

.. include:: text/git-php-examples.txt

List Metrics
============
   
**Imports**

.. literalinclude::  cloudwatch.php.list_metrics.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudwatch.php.list_metrics.main.txt
   :language: PHP


Retrieve Alarms for a Metric
============================

**Imports**

.. literalinclude::  cloudwatch.php.describe_alarms_metric.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudwatch.php.describe_alarms_metric.main.txt
   :language: PHP

Get Metric Statistics
=====================

**Imports**

.. literalinclude::  cloudwatch.php.get_metric_stats.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudwatch.php.get_metric_stats.main.txt
   :language: PHP