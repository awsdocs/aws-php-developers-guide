.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################################
Using Elastic IP Addresses with |EC2| with |sdk-php| Version 3
##############################################################

.. meta::
   :description: Describe Amazon EC2 instances and acquire, associate, and release Elastic IP addresses using the AWS SDK for PHP version 3.
   :keywords: Amazon EC2 code examples for PHP

An Elastic IP address is a static IP address designed for dynamic cloud computing. An Elastic IP address is associated with your AWS account. It's a public IP address, 
which is reachable from the internet. If your instance does not have a public IP address, you can associate an Elastic IP address with your instance to enable communication with the internet.

The following examples show how to:

* Describe one or more of your instances using :aws-php-class:`DescribeInstances </api-ec2-2016-11-15.html#describeinstances>`.
* Acquire an Elastic IP address using :aws-php-class:`AllocateAddress </api-ec2-2016-11-15.html#allocateaddress>`.
* Associate an Elastic IP address with an instance using :aws-php-class:`AssociateAddress </api-ec2-2016-11-15.html#associateaddress>`.
* Release an Elastic IP address using :aws-php-class:`ReleaseAddress </api-ec2-2016-11-15.html#releaseaddress>`.

.. include:: text/git-php-examples.txt

Describe an Instance
====================

**Imports**

.. literalinclude::  example_code/ec2/DescribeInstances.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/DescribeInstances.php
   :lines: 30-38
   :language: php


Allocate and Associate an Address
=================================

**Imports**

.. literalinclude::  example_code/ec2/AllocateAddresses.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/AllocateAddresses.php
   :lines: 30-49
   :language: php


Release an Address
==================

**Imports**

.. literalinclude::  example_code/ec2/ReleaseAddress.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/ReleaseAddress.php
   :lines: 30-48
   :language: php
