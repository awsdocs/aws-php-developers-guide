.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################################
Publishing Custom Metrics in |CWlong| with |sdk-php| Version 3
##############################################################

.. meta::
   :description: Publish metric data and create alarms for Amazon CloudWatch using the AWS SDK for PHP version 3.
   :keywords: Amazon CloudWatch code examples for PHP

Metrics are data about the performance of your systems. An alarm watches a single metric over a time period you
specify. It performs one or more actions based on the value of the metric, relative to a given threshold over a number of time periods.

The following examples show how to:

* Publish metric data using :aws-php-class:`PutMetricData </api-monitoring-2010-08-01.html#putmetricdata>`.
* Create an alarm using :aws-php-class:`PutMetricAlarm </api-monitoring-2010-08-01.html#putmetricalarm>`.

.. include:: text/git-php-examples.txt

Publish Metric Data
===================

**Imports**

.. literalinclude::  example_code/cloudwatch/PutMetricData.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/PutMetricData.php
   :lines: 31-54
   :language: php

Create an Alarm
===============

**Imports**

.. literalinclude::  example_code/cloudwatch/PutMetricAlarm.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/PutMetricAlarm.php
   :lines: 31-63
   :language: php