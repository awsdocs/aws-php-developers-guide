.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################################
Managing |EC2| Instances Using the |sdk-php| Version 3
#######################################################

.. meta::
   :description: Engage with Amazon EC2 instances using the AWS SDK for PHP version 3.
   :keywords: Amazon EC2 code examples for PHP

The following examples show how to:

* Describe |EC2| instances using :aws-php-class:`DescribeInstances <api-ec2-2016-11-15.html#describeinstances>`.
* Enable detailed monitoring for a running instance using :aws-php-class:`MonitorInstances <api-ec2-2016-11-15.html#monitorinstances>`.
* Disable monitoring for a running instance using :aws-php-class:`UnmonitorInstances <api-ec2-2016-11-15.html#unmonitorinstances>`.
* Start an |EBS|-backed AMI that you've previously stopped, using :aws-php-class:`StartInstances <api-ec2-2016-11-15.html#startinstances>`.
* Stop an |EBS|-backed instance using :aws-php-class:`StopInstances <api-ec2-2016-11-15.html#stopinstances>`.
* Request a reboot of one or more instances using :aws-php-class:`RebootInstances <api-ec2-2016-11-15.html#rebootinstances>`.

.. include:: text/git-php-examples.txt

Describe Instances
==================

**Imports**

.. literalinclude:: ec2.php.describe_instances.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.describe_instances.main.txt
   :language: PHP

Enable and Disable Monitoring
=============================


**Imports**

.. literalinclude:: ec2.php.monitor_instances.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.monitor_instances.main.txt
   :language: PHP

Start and Stop an Instance
==========================

**Imports**

.. literalinclude:: ec2.php.start_and_stop_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.start_and_stop_instance.main.txt
   :language: PHP


Reboot an Instance
==================

**Imports**

.. literalinclude:: ec2.php.reboot_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.reboot_instance.main.txt
   :language: PHP

