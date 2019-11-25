.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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
   :description: Using Access Point ARNs in S3 requests with the AWS SDK for PHP version 3.
   :keywords: Amazon S3 code examples for PHP

S3 introduced access points, a new way to interact with S3 buckets. Access Points can have unique policies and configuration applied to them instead of directly to the bucket. The AWS SDK for PHP allows you to use access point ARNs in the bucket field for API operations instead of specifying bucket name explicitly. More details on how S3 access points and ARNs work can be found `here <https://todo/when/docs/are/up`_. The following examples show how to:

* Use :aws-php-class:`GetObject <api-s3-2006-03-01.html#getobject>` with an access point ARN to fetch an object from a bucket.
* Use :aws-php-class:`PutObject <api-s3-2006-03-01.html#putobject>` with an access point ARN to add an object to a bucket.
* Configure the S3 client to use the ARN region instead of the client region.

.. include:: text/git-php-examples.txt

**Imports**

.. literalinclude:: s3.php.list_buckets.import.txt
   :language: php


Get Object
==========

First create an AWS.S3 client service that specifies the AWS region and version. Then call the ``getObject`` method with your key and an S3 access point ARN in the ``Bucket`` field, which will fetch the object from the bucket associated with that access point.

**Sample Code**

.. code-block:: php

    $s3 = new S3Client([
        'version'     => 'latest',
        'region'      => 'us-west-2',
    ]);
    $result = $s3->getObject([
        'Bucket' => 'arn:aws:s3:us-west-2:123456789012:accesspoint:endpoint-name',
        'Key' => 'MyKey'
    ]);


Put an Object in a Bucket
=========================

First create an AWS.S3 client service that specifies the AWS Region and version. Then call the ``putObject`` method with the desired key, the body or source file, and an S3 access point ARN in the ``Bucket`` field, which will put the object in the bucket associated with that access point.

**Sample Code**

.. code-block:: php

    $s3 = new S3Client([
        'version'     => 'latest',
        'region'      => 'us-west-2',
    ]);
    $result = $s3->putObject([
        'Bucket' => 'arn:aws:s3:us-west-2:123456789012:accesspoint:endpoint-name',
        'Key' => 'MyKey',
        'Body' => 'MyBody'
    ]);


Configure the S3 client to use the ARN region instead of the client region
==========================================================================

When using an S3 access point ARN in an S3 client operation, by default the client will make sure that the ARN region matches the client region, throwing an exception if it does not. This behavior can be changed to accept the ARN region over the client region by setting the ``use_arn_region`` configuration option to ``true``. By default, the option is set to ``false``.

**Sample Code**

.. code-block:: php

    $s3 = new S3Client([
        'version'        => 'latest',
        'region'         => 'us-west-2',
        'use_arn_region' => true
    ]);

The client will also check an environment variable and a config file option, in the following order of priority:

1. The client option ``use_arn_region``, as in the above example.

2. The environment variable ``AWS_S3_USE_ARN_REGION``

.. code-block:: ini

    export AWS_S3_USE_ARN_REGION=true

3. The config variable ``s3_use_arn_region`` in the AWS shared configuration file (by default in :file:`~/.aws/config`).

.. code-block:: ini

    [default]
    s3_use_arn_region = true
