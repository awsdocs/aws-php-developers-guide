.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

====================================
Using Regions and Availability Zones
====================================

.. meta::
   :description: Describe AWS Regions and Availability Zones for |EC2|.
   :keywords: |EC2|, |sdk-php| examples

|EC2| is hosted in multiple locations worldwide. These locations are composed of regions and Availability Zones. Each region is a separate geographic area. Each region has multiple, isolated locations known as Availability Zones. |EC2| provides the ability to place instances and data in multiple locations.

The examples below show how to:

* Describe the Availability Zones that are available to you using :aws-php-class:`DescribeAvailabilityZones </api-ec2-2016-11-15.html#describeavailabilityzones>`.
* Describe regions that are currently available to you using :aws-php-class:`DescribeRegions </api-ec2-2016-11-15.html#describeregions>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials, as described in :doc:`guide_credentials`.

Describe Availability Zones
---------------------------

**Imports**

.. literalinclude::  example_code/ec2/DescribeAvailabilityZones.php
   :lines: 15-17
   :language: PHP

**Code**

.. literalinclude:: example_code/ec2/DescribeAvailabilityZones.php
   :lines: 26-34
   :language: php

Describe Regions
----------------

**Imports**

.. literalinclude::  example_code/ec2/DescribeRegions.php
   :lines: 15-17
   :language: PHP

**Code**

.. literalinclude:: example_code/ec2/DescribeRegions.php
   :lines: 26-34
   :language: php
