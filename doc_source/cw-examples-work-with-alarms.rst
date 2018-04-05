.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

============================
Working with |CWlong| Alarms
============================

.. meta::
   :description: Create Amazon CloudWatch alarms that automatically stop, terminate, reboot, or recover Amazon EC2 instances using the AWS SDK for PHP.
   :keywords: Amazon CloudWatch code examples for PHP

An |CWlong| alarm watches a single metric over a time period you specify. It performs one or more actions based on the value of
the metric relative to a given threshold over a number of time periods.

The following examples show how to:

* Describe an alarm using :aws-php-class:`DescribeAlarms <api-monitoring-2010-08-01.html#describealarms>`.
* Create an alarm using :aws-php-class:`PutMetricAlarm <api-monitoring-2010-08-01.html#putmetricalarm>`.
* Delete an alarm using :aws-php-class:`DeleteAlarms <api-monitoring-2010-08-01.html#deletealarms>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials. See :doc:`guide_credentials`.

Describe Alarms
---------------

**Imports**

.. literalinclude::  example_code/cloudwatch/DescribeAlarms.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/DescribeAlarms.php
   :lines: 31-46
   :language: php


Create an Alarm
---------------

**Imports**

.. literalinclude::  example_code/cloudwatch/PutMetricAlarm.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/PutMetricAlarm.php
   :lines: 31-63
   :language: php

Delete Alarms
-------------

**Imports**

.. literalinclude::  example_code/cloudwatch/DeleteAlarms.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/DeleteAlarms.php
   :lines: 31-47
   :language: php
