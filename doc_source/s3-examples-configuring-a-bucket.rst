.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

========================
Configuring |S3| Buckets
========================

.. meta::
   :description: Get or set CORS configuration for an Amazon S3 bucket using the AWS SDK for PHP.
   :keywords: Amazon S3 code examples for PHP

Cross-origin resource sharing (CORS) defines a way for client web applications that are loaded in one domain to interact with resources in a different domain. With CORS support in |S3|,
you can build rich client-side web applications with |S3| and selectively allow cross-origin access to your |S3| resources.

For more information about using CORS configuration with an |S3| bucket, see :S3-dg:`Cross-Origin Resource Sharing (CORS) <cors>`.

The following examples show how to:

* Get the CORS configuration for a bucket using :aws-php-class:`GetBucketCors <api-s3-2006-03-01.html#getbucketcors>`.
* Set the CORS configuration for a bucket using :aws-php-class:`PutBucketCors <api-s3-2006-03-01.html#putbucketcors>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials. See :doc:`guide_credentials`.

Get the CORS Configuration
--------------------------

Create a PHP file with following code. First create an AWS.S3 client service, then call the ``getBucketCors`` method and specify the bucket whose CORS configuration you want.

The only parameter required is the name of the selected bucket. If the bucket currently has a CORS configuration, that configuration is returned by |S3| as a
:aws-php-class:`CORSRules object </api-s3-2006-03-01.html#shape-corsrule>`.

**Imports**

.. literalinclude::  example_code/s3/GetBucketCors.php
   :lines: 19-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/s3/GetBucketCors.php
   :lines: 31-46
   :language: php

Set the CORS Configuration
--------------------------

Create a PHP file with following code. First create an AWS.S3 client service. Then call the ``putBucketCors`` method and specify the bucket whose CORS configuration you want
to set, and the CORSConfiguration as a :aws-php-class:`CORSRules JSON object </api-s3-2006-03-01.html#shape-corsrule>`.

**Sample Code**

.. literalinclude:: example_code/s3/PutBucketCors.php
   :lines: 38-57
   :language: php