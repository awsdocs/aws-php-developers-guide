.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#############################################################################
Manage |CFlong| Distributions Using the |AKS| API and the |sdk-php| Version 3
#############################################################################

.. meta::
   :description: Amazon CloudFront code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon CloudFront code examples for PHP, Amazon CloudFront Data for PHP.

|CFlong| distributes your static and dynamic web content from Amazon services like |S3| and |EC2| to world wide data centers called edge locations. 
When customers request content from your website, it is stored in the closest edge location, which reduces the latency of similar requests in that area. 

Each distribution specifies where your content is located, and how it should be distributed. This article focuses on distributions for static and 
dynamic files like html, css, json, and image files. For more information about  video on demand files for multimedia streaming and live events 
checkout out :CF-dg:`On-Demand and Live Streaming Video with CloudFront <on-demand-streaming-video>`.

The following examples show how to:

* Create a Distribution using :aws-php-class:`CreateDistribution <api-cloudfront-2018-11-05.html#createdistribution>`.
* Get Distribution using :aws-php-class:`GetDistribution <api-cloudfront-2018-11-05.html#getdistribution>`.
* List Distributions using :aws-php-class:`ListDistributions <api-cloudfront-2018-11-05.html#listdistributions>`.
* Update Distributions using :aws-php-class:`UpdateDistributions <api-cloudfront-2018-11-05.html#updatedistribution>`.
* Disable Distributions using :aws-php-class:`DisableDistribution <api-cloudfront-2018-11-05.html#disabledistribution>`.
* Delete Distributions using :aws-php-class:`DeleteDistributions <api-cloudfront-2018-11-05.html#deletedistribution>`.

.. include:: text/git-php-examples.txt

For more information about using |CFlong| , see the |CF-dg|_.

Create |CF| Distribution
========================

Create a distribution from an S3 bucket. In the example below, optional parameters are commented out, but display the default values. 
To add additional customizations to your distribution, uncomment both the value and the parameter inside :code:`$distribution`. 

To create a |CF| distribution, use the :CF-api:`CreateDistribution <API_CreateDistribution>` operation.

**Imports**

.. literalinclude::  cloudfront.php.creates3distribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.creates3distribution.main.txt
   :language: PHP

Retrieve a |CF| Distribution
============================

To retrieve the status and details of a specified |CF| distribution, use the :CF-api:`GetDistribution <API_GetDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.getdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.getdistribution.main.txt
   :language: php

List |CF| Distributions
========================

Get a list of existing |CF| distributions in the given region from your current account with the :CF-api:`ListDistributions <API_ListDistributions>` operation.

**Imports**

.. literalinclude:: cloudfront.php.listdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.listdistribution.main.txt
   :language: php

   
Update a |CF| Distribution
============================

Updating a |CF| distribution, is similar to creating a distribution. However more fields are required, and all values must be included when update a distribution.
To make changes to an existing distribution, we recommend that you first retrieve the existing distribution, and update the values you want to change in the :code:`$distribution` array.

To update a specified |CF| distribution, use the :CF-api:`UpdateDistribution <API_UpdateDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.updatedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.updatedistribution.main.txt
   :language: php

Disable a |CF| Distribution
============================

If you want to deactivate or remove a distribution, change its status from deployed to disabled.

To disable the specified |CF| distribution, use the :CF-api:`DisableDistribution <API_DisableDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.disabledistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.disabledistribution.main.txt
   :language: php

Delete a |CF| Distribution
============================

Once a distribution is in status disabled, you can delete the distribution. 

To remove a specified |CF| distribution, use the :CF-api:`DeleteDistribution <API_DeleteDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.deletedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.deletedistribution.main.txt
   :language: php