.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################################
Managing |CFlong| Invalidations Using the |CF| API and the |sdk-php| Version 3
###############################################################################

.. meta::
   :description: Amazon CloudFront code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon CloudFront code examples for PHP, Amazon CloudFront Data for PHP.


|CFlong| caches copies of static and dynamic files in worldwide edge locations. To remove or update a 
file on all edge locations, create an invalidation for each file or for a group of files.

Each calendar month, your first 1,000 invalidations are free. To learn more about removing content from 
a |CF| edge location, see :CF-dg:`Invalidating Files <Invalidation>`.

The following examples show how to:

* Create a distribution invalidation using :aws-php-class:`CreateInvalidation <api-cloudfront-2018-11-05.html#createinvalidation>`.
* Get a distribution invalidation using :aws-php-class:`GetInvalidation <api-cloudfront-2018-11-05.html#getinvalidation>`.
* List distributions using :aws-php-class:`ListInvalidations <api-cloudfront-2018-11-05.html#listinvalidations>`.

.. include:: text/git-php-examples.txt

For more information about using |CFlong|, see the |CF-dg|_.

Create a Distribution Invalidation
==================================

Create a |CF| distribution invalidation by specifying the path location for the files you need to remove.
This example invalidates all files in the distribution, but you can identify specific files under :code:`Items`.

To create a |CF| distribution invalidation, use the :CF-api:`CreateInvalidation <API_CreateInvalidation>` operation.

**Imports**

.. literalinclude:: cloudfront.php.createinvalidation.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.createinvalidation.main.txt
   :language: php


Get a Distribution Invalidation
===============================

To retrieve the status and details about a |CF| distribution invalidation, use the :CF-api:`GetInvalidation <API_GetInvalidation>` operation.

**Imports**

.. literalinclude:: cloudfront.php.getinvalidation.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.getinvalidation.main.txt
   :language: php

List Distribution Invalidations
===============================

To list all current |CF| distribution invalidations, use the :CF-api:`ListInvalidations <API_ListInvalidations>` operation.

**Imports**

.. literalinclude:: cloudfront.php.listinvalidation.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.listinvalidation.main.txt
   :language: php
