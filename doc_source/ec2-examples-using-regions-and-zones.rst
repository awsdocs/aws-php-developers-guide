.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################################################
Using Regions and Availability Zones for |EC2| with |sdk-php| Version 3
#######################################################################

.. meta::
   :description: Describe AWS Regions and Availability Zones for Amazon EC2 using the AWS SDK for PHP version 3.
   :keywords: Amazon EC2 code examples for PHP

|EC2| is hosted in multiple locations worldwide. These locations are composed of AWS Regions and Availability Zones. Each Region is a separate geographic area, 
with multiple isolated locations known as Availability Zones. |EC2| provides the ability to place instances and data in multiple locations.

The following examples show how to:

* Describe the Availability Zones that are available to you using :aws-php-class:`DescribeAvailabilityZones </api-ec2-2016-11-15.html#describeavailabilityzones>`.
* Describe AWS Regions that are currently available to you using :aws-php-class:`DescribeRegions </api-ec2-2016-11-15.html#describeregions>`.

.. include:: text/git-php-examples.txt

Describe Availability Zones
===========================

**Imports**

.. literalinclude:: ec2.php.describe_availability_zones.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.describe_availability_zones.main.txt
   :language: PHP

Describe Regions
================

**Imports**

.. literalinclude:: ec2.php.describe_regions.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: ec2.php.describe_regions.main.txt
   :language: PHP

