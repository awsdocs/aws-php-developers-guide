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

|CFlong| 

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

Create a distribution.

To create a |CF| distribution, use the :CF-api:`CreateDistribution <API_CreateDistribution>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/CreateDistributionS3.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/CreateDistributionS3.php
   :lines: 34-
   :language: php

Retrieve a |CF| Distribution
============================

To retrieve the details of the specified |CF| distribution, use the :CF-api:`GetDistribution <API_GetDistribution>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/GetDistribution.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/GetDistribution.php
   :lines: 34-
   :language: php

List |CF| Distributions
========================

To list the existing |CF| distributions in the given accounts, use the :CF-api:`ListDistributions <API_ListDistributions>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/ListDistributions.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/ListDistributions.php
   :lines: 34-
   :language: php

Update a |CF| Distribution
============================

To update a specified |CF| distribution, use the :CF-api:`UpdateDistribution <API_UpdateDistribution>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/UpdateDistributionS3.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/UpdateDistributionS3.php
   :lines: 34-
   :language: php

Disable a |CF| Distribution
============================

To disable the specified |CF| distribution, use the :CF-api:`DisableDistribution <API_DisableDistribution>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/DisableDistributionS3.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/DisableDistributionS3.php
   :lines: 34-
   :language: php

Delete a |CF| Distribution
============================

To remove a specified |CF| distribution, use the :CF-api:`DeleteDistribution <API_DeleteDistribution>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/DeleteDistribution.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/DeleteDistribution.php
   :lines: 34-
   :language: php