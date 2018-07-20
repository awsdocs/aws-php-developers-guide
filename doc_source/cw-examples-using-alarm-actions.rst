.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################################
Using Alarm Actions with |CWlong| Alarms with |sdk-php| Version 3
#################################################################

.. meta::
   :description: Create Amazon CloudWatch alarms that automatically stop, terminate, reboot, or recover EC2 instances using the AWS SDK for PHP version 3.
   :keywords: Amazon CloudWatch code examples for PHP

Use alarm actions to create alarms that automatically stop, terminate, reboot, or recover your |EC2| instances. You can use the stop or terminate actions when you no longer need an instance to be running. You can use the reboot and recover actions to automatically reboot those instances.

The following examples show how to:

* Enable actions for specified alarms using :aws-php-class:`EnableAlarmActions </api-monitoring-2010-08-01.html#enablealarmactions>`.
* Disable actions for specified alarms using :aws-php-class:`DisableAlarmActions </api-monitoring-2010-08-01.html#disablealarmactions>`.

.. include:: text/git-php-examples.txt

Enable Alarm Actions
====================

**Imports**

.. literalinclude::  example_code/cloudwatch/EnableAlarmActions.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/EnableAlarmActions.php
   :lines: 31-47
   :language: php

Disable Alarm Actions
=====================

**Imports**

.. literalinclude::  example_code/cloudwatch/DisableAlarmActions.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudwatch/DisableAlarmActions.php
   :lines: 31-47
   :language: php
