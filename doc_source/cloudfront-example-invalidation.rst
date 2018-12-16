.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################################
Manage |CFlong| Invalidations Using the |AKS| API and the |sdk-php| Version 3
##########################################################################

.. meta::
   :description: Amazon CloudFront code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon CloudFront code examples for PHP, Amazon CloudFront Data for PHP.


|CFSlong| 

The following examples show how to:

* Create a Distribution Invalidation using :aws-php-class:`CreateInvalidation <api-cloudfront-2018-11-05.html#createinvalidation>`.
* Get Invalidation using :aws-php-class:`GetInvalidation <api-cloudfront-2018-11-05.html#getinvalidation>`.
* List Distributions using :aws-php-class:`ListInvalidations <api-cloudfront-2018-11-05.html#listinvalidations>`.

.. include:: text/git-php-examples.txt

For more information about using |CFSlong| , see the |CF-dg|_.

Create a Distribution Invalidation
==================================

Create a |CF| Distribution Invalidation.

To create a |CF| distribution invalidation, use the :CF-api:`CreateInvalidation <API_CreateInvalidation>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/CreateInvalidation.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/CreateInvalidation.php
   :lines: 34-
   :language: php

Get a Distribution Invalidation
===============================

To retrieve a |CF| distribution invalidation, use the :CF-api:`GetInvalidation <API_GetInvalidation>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/GetInvalidation.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/GetInvalidation.php
   :lines: 34-
   :language: php

List Distribution Invalidations
===============================

To list all |CF| distribution invalidations, use the :CF-api:`ListInvalidations <API_ListInvalidations>` operation.

**Imports**

.. literalinclude::  example_code/cloudfront/ListInvalidations.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/cloudfront/ListInvalidations.php
   :lines: 34-
   :language: php