.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################################
Working with Security Groups in |EC2| with |sdk-php| Version 3
##############################################################

.. meta::
   :description: Create, describe, and delete security groups for Amazon EC2 using the AWS SDK for PHP version 3.
   :keywords: Amazon EC2 code examples for PHP

An |EC2| security group acts as a virtual firewall that controls the traffic for one or more instances. You add rules to each security group to allow traffic to or from its associated instances. You can modify the rules for a security group at any time. The new rules are automatically applied to all instances that are associated with the security group.

The following examples show how to:

* Describe one or more of your security groups using :aws-php-class:`DescribeSecurityGroups </api-ec2-2016-11-15.html#describesecuritygroups>`.
* Add an ingress rule to a security group using :aws-php-class:`AuthorizeSecurityGroupIngress </api-ec2-2016-11-15.html#authorizesecuritygroupingress>`.
* Create a security group using :aws-php-class:`CreateSecurityGroup </api-ec2-2016-11-15.html#createsecuritygroup>`.
* Delete a security group using :aws-php-class:`DeleteSecurityGroup </api-ec2-2016-11-15.html#deletesecuritygroup>`.

.. include:: text/git-php-examples.txt

Describe Security Groups
========================

**Imports**

.. literalinclude:: example_code/ec2/DescribeSecurityGroups.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/DescribeSecurityGroups.php
   :lines: 30-38
   :language: php

Add an Ingress Rule
====================

**Imports**

.. literalinclude:: example_code/ec2/AuthorizeSecurityGroupIngress.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/AuthorizeSecurityGroupIngress.php
   :lines: 30-41
   :language: php


Create a Security Group
=======================

**Imports**

.. literalinclude:: example_code/ec2/CreateSecurityGroup.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/CreateSecurityGroup.php
   :lines: 30-47
   :language: php

Delete a Security Group
=======================

**Imports**

.. literalinclude:: example_code/ec2/DeleteSecurityGroup.php
   :lines: 19-21
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ec2/DeleteSecurityGroup.php
   :lines: 30-42
   :language: php
