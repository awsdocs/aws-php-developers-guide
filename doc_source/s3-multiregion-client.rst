.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################################
|S3| Multi-Region Client with |sdk-php| Version 3 
#######################################################

.. meta::
   :description: Create a multi-region Amazon S3 client using the AWS SDK for PHP version 3 .
   :keywords: Amazon S3, AWS SDK for PHP version 3 examples, Amazon S3 for PHP version 3 code examples


The |sdk-php| Version 3 provides a generic multi-region client that can be used with
any service. This enables users to specify which AWS Region to send a command to by
providing an ``@region`` input parameter to any command. In addition, the SDK
provides a multi-region client for |S3| that responds intelligently to
specific |S3| errors and reroutes commands accordingly. This enables users to use
the same client to talk to multiple Regions. This is a particularly useful feature for
users of the :doc:`s3-stream-wrapper`, whose buckets reside in multiple
Regions.

Basic Usage
===========

The basic usage pattern of an |S3| client is the same whether using a
standard S3 client or its multi-region counterpart. The only usage difference at
the command level is that an AWS Region can be specified using the ``@region`` input
parameter.

.. code-block:: php

    // Create a multi-region S3 client
    $s3Client = (new \Aws\Sdk)->createMultiRegionS3(['version' => 'latest']);

    // You can also use the client constructor
    $s3Client = new \Aws\S3\S3MultiRegionClient([
        'version' => 'latest',
        // Any Region specified while creating the client will be used as the
        // default Region
        'region' => 'us-west-2',
    ]);

    // Get the contents of a bucket
    $objects = $s3Client->listObjects(['Bucket' => $bucketName]);

    // If you would like to specify the Region to which to send a command, do so
    // by providing an @region parameter
    $objects = $s3Client->listObjects([
        'Bucket' => $bucketName,
        '@region' => 'eu-west-1',
    ]);

.. important::

    When using the multi-region |S3| client, you will not encounter any permanent
    redirect exceptions. A standard |S3| client will throw an instance of
    ``Aws\S3\Exception\PermanentRedirectException`` when a command is sent to
    the wrong Region. A multi-region client will instead redispatch the command
    to the correct Region.

Bucket Region Cache
===================

|S3| multi-region clients maintain an internal cache of the AWS Regions in
which given buckets reside. By default, each client has its own in-memory cache.
To share a cache between clients or processes, supply an instance of
``Aws\CacheInterface`` as the ``bucket_region_cache`` option to your
multi-region client.

.. code-block:: php

    use Aws\DoctrineCacheAdapter;
    use Aws\Sdk;
    use Doctrine\Common\Cache\ApcuCache;

    $sdk = new Aws\Sdk([
        'version' => 'latest',
        'region' => 'us-west-2',
        'S3' => [
            'bucket_region_cache' => new DoctrineCacheAdapter(new ApcuCache),
        ],
    ]);
