.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##################################################################
Creating and Using |S3| Buckets with the AWS SDK for PHP Version 3
##################################################################

.. meta::
   :description: Describes how to use Amazon S3 buckets with the AWS SDK for PHP version 3.
   :keywords: Amazon S3 code examples for PHP

The following examples show how to:

* Return a list of buckets owned by the authenticated sender of the request using :aws-php-class:`ListBuckets <api-s3-2006-03-01.html#listbuckets>`.
* Create a new bucket using :aws-php-class:`CreateBucket <api-s3-2006-03-01.html#createbucket>`.
* Add an object to a bucket using :aws-php-class:`PutObject <api-s3-2006-03-01.html#putobject>`.

.. include:: text/git-php-examples.txt

**Imports**

.. literalinclude::  example_code/s3/ListBuckets.php
   :lines: 20-23
   :language: PHP


List Buckets
============

Create a PHP file with the following code. First create an AWS.S3 client service that specifies the AWS Region and version. Then call the ``listBuckets`` method, which returns all |s3| buckets owned by the sender of the request as an array of Bucket structures.

**Sample Code**

.. literalinclude:: example_code/s3/ListBuckets.php
   :lines: 32-
   :language: php

Create a Bucket
===============

Create a PHP file with the following code. First create an AWS.S3 client service that specifies the AWS Region and version. Then call the ``createBucket`` method with an array as the parameter. The only required field is the key 'Bucket', with a string value for the bucket name to create. However, you can specify the AWS Region with the 'CreateBucketConfiguration' field.  If successful, this method returns the 'Location' of the bucket.

**Sample Code**

.. literalinclude:: example_code/s3/CreateBucket.php
   :lines: 31-
   :language: php

Put an Object in a Bucket
=========================

To add files to your new bucket, create a PHP file with the following code.

In your command line, execute this file and pass in the name of the bucket where you want to upload your file as a string, followed by the full file path to the file to upload.

**Sample Code**

.. literalinclude:: example_code/s3//PutObject.php
   :lines: 31-
   :language: php
