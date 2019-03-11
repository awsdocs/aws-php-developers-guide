.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################################
Managing |CFlong| Distributions Using the |__| API and the |sdk-php| Version 3
###############################################################################

.. meta::
   :description: Amazon CloudFront code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon CloudFront code examples for PHP, Amazon CloudFront Data for PHP.

|long| Description


The following examples show how to:

* Create _____________ using :aws-php-class:`Create_________ <api-cloudfront-2018-11-05.html#create______>`.
* Get ________________ using :aws-php-class:`Get____________ <api-cloudfront-2018-11-05.html#get_________>`.
* List _______________ using :aws-php-class:`List___________ <api-cloudfront-2018-11-05.html#list________>`.
* Update _____________ using :aws-php-class:`Update_________ <api-cloudfront-2018-11-05.html#update______>`.
* Disable ____________ using :aws-php-class:`Disable________ <api-cloudfront-2018-11-05.html#disable_____>`.
* Delete _____________ using :aws-php-class:`Delete_________ <api-cloudfront-2018-11-05.html#delete______>`.

.. include:: text/git-php-examples.txt

For more information about using |CFlong|, see the |CF-dg|_.

Create a |__| Distribution
==========================

To create a |__|, use the :CF-api:`CreateDistribution <API_CreateDistribution>` operation.

**Imports**

.. literalinclude::  cloudfront.php.creates3distribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.creates3distribution.main.txt
   :language: PHP

Retrieve a |__| Distribution
============================

To retrieve |__| distribution, use the :CF-api:`GetDistribution <API_GetDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.getdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.getdistribution.main.txt
   :language: php

List |__| Distributions
=======================

Get a list of existing |__| using the :CF-api:`ListDistributions <API_ListDistributions>` operation.

**Imports**

.. literalinclude:: cloudfront.php.listdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.listdistribution.main.txt
   :language: php


Update a |__| Distribution
==========================

To update a specified |__| distribution, use the :CF-api:`UpdateDistribution <API_UpdateDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.updatedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.updatedistribution.main.txt
   :language: php

Disable a |__| Distribution
============================

To disable the specified |__| distribution, use the :CF-api:`DisableDistribution <API_DisableDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.disabledistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.disabledistribution.main.txt
   :language: php

Delete a |__| Distribution
==========================

To remove a specified |__| distribution, use the :CF-api:`DeleteDistribution <API_DeleteDistribution>` operation.

**Imports**

.. literalinclude:: cloudfront.php.deletedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.deletedistribution.main.txt
   :language: php