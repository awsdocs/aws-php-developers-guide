.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################################
Managing |CFlong| Distributions Using the |CF| API and the |sdk-php| Version 3
###############################################################################

.. meta::
   :description: Amazon CloudFront code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon CloudFront code examples for PHP, Amazon CloudFront Data for PHP.

|CFlong| caches content in worldwide edge locations to speed up distribution of static and dynamic files that you store 
on your own server, or on an Amazon service like |S3| and |EC2|. When users request content from your website, |CF| serves 
it from the closest edge location, if the file is cached there. Otherwise |CF| retrieves a copy of the file, serves it, 
and then caches it for the next request. Caching content at an edge location reduces the latency of similar user 
requests in that area.

For each |CF| distribution that you create, you specify where the content is located and how to distribute it when users 
make requests. This topic focuses on distributions for static and dynamic files such as HTML, CSS, JSON, and image files. F
or information about using |CF| with video on demand, 
see :CF-dg:`On-Demand and Live Streaming Video with CloudFront <on-demand-streaming-video>`.

The following examples show how to:

* Create a distribution using :aws-php-class:`CreateDistribution <api-cloudfront-2018-11-05.html#createdistribution>`.
* Get a distribution using :aws-php-class:`GetDistribution <api-cloudfront-2018-11-05.html#getdistribution>`.
* List distributions using :aws-php-class:`ListDistributions <api-cloudfront-2018-11-05.html#listdistributions>`.
* Update distributions using :aws-php-class:`UpdateDistributions <api-cloudfront-2018-11-05.html#updatedistribution>`.
* Disable distributions using :aws-php-class:`DisableDistribution <api-cloudfront-2018-11-05.html#disabledistribution>`.
* Delete distributions using :aws-php-class:`DeleteDistributions <api-cloudfront-2018-11-05.html#deletedistribution>`.

.. include:: text/git-php-examples.txt

For more information about using |CFlong|, see the |CF-dg|_.

Create a |CF| Distribution
==========================

Create a distribution from an |S3| bucket. In the following example, optional parameters are commented out, but default values are displayed.
To add customizations to your distribution, uncomment both the value and the parameter inside :code:`$distribution`.

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
=======================

Get a list of existing |CF| distributions in the specified AWS Region from your current account using the :CF-api:`ListDistributions <API_ListDistributions>` operation.

**Imports**

.. literalinclude:: cloudfront.php.listdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.listdistribution.main.txt
   :language: php


Update a |CF| Distribution
==========================

Updating a |CF| distribution is similar to creating a distribution. However, when you update a distribution, more fields are required and all values must be included.
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

To deactivate or remove a distribution, change its status from deployed to disabled.

To disable the specified |CF| distribution, use the :CF-api:`DisableDistribution <API_DisableDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.disabledistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.disabledistribution.main.txt
   :language: php

Delete a |CF| Distribution
==========================

Once a distribution is in a disabled status, you can delete the distribution.

To remove a specified |CF| distribution, use the :CF-api:`DeleteDistribution <API_DeleteDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.deletedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.deletedistribution.main.txt
   :language: php
