.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#####################################################
Working with |CWlong| Alarms with |sdk-php| Version 3
#####################################################

.. meta::
   :description: Create Amazon CloudWatch alarms that automatically stop, terminate, reboot, or recover Amazon EC2 instances using the AWS SDK for PHP version 3.
   :keywords: Amazon CloudWatch code examples for PHP

An |CWlong| alarm watches a single metric over a time period you specify. It performs one or more actions based on the value of
the metric relative to a given threshold over a number of time periods.

The following examples show how to:

* Describe an alarm using :aws-php-class:`DescribeAlarms <api-monitoring-2010-08-01.html#describealarms>`.
* Create an alarm using :aws-php-class:`PutMetricAlarm <api-monitoring-2010-08-01.html#putmetricalarm>`.
* Delete an alarm using :aws-php-class:`DeleteAlarms <api-monitoring-2010-08-01.html#deletealarms>`.

.. include:: text/git-php-examples.txt

Describe Alarms
===============

**Imports**

.. literalinclude:: cloudwatch.php.describe_alarms.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudwatch.php.describe_alarms.main.txt
   :language: PHP


Create an Alarm
===============

**Imports**

.. literalinclude:: cloudwatch.php.put_metric_alarm.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudwatch.php.put_metric_alarm.main.txt
   :language: PHP

Delete Alarms
=============

**Imports**

.. literalinclude:: cloudwatch.php.delete_alarm.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudwatch.php.delete_alarm.main.txt
   :language: PHP
