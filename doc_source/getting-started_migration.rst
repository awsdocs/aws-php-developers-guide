.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

=========================================
Migrating from Version 2 of the |sdk-php|
=========================================

.. meta::
   :description: Shows how to migrate to AWS SDK for PHP version 3 from version 2.
   :keywords: AWS SDK for PHP version 2, AWS SDK for PHP v2, AWS SDK for PHP 2, migrate to version 3

This topic shows how to migrate your code to use version 3 of the |sdk-php|
and how the new version differs from version 2 of the SDK.

.. note::

    The basic usage pattern of the SDK (i.e., ``$result = $client->operation($params);``)
    has not changed from version 2 to version 3, which should result in a smooth migration.

Introduction
------------

Version 3 of the |sdk-php| represents a significant effort to improve the capabilities
of the SDK, incorporate over two years of customer feedback, upgrade our
dependencies, improve performance, and adopt the latest PHP standards.

What's New in Version 3?
------------------------
Version 3 of the |sdk-php| follows the `PSR-4 and PSR-7 standards <http://php-fig.org>`_ and 
will follow the `SemVer <http://semver.org/>`_ standard going forward. 

Other new features include 

- Middleware system for customizing service client behavior
- Flexible *paginators* for iterating through paginated results
- Ability to query data from *result* and *paginator* objects with *JMESPath*
- Easy debugging via the ``'debug'`` configuration option


Decoupled HTTP layer
~~~~~~~~~~~~~~~~~~~~

- `Guzzle 6 <http://guzzlephp.org>`_ is used by default to send requests, but
  Guzzle 5 is also supported.
- The SDK will work in environments where cURL is not available.
- Custom HTTP handlers are also supported.

Asynchronous requests
~~~~~~~~~~~~~~~~~~~~~

- Features like *waiters* and *multipart uploaders* can also be used
  asynchronously.
- Asynchronous workflows can be created using *promises* and *coroutines*.
- Performance of concurrent or batched requests is improved.



What's Different from Version 2?
--------------------------------

Project Dependencies are Updated
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The dependencies of the SDK have changed in this version.

- The SDK now requires PHP 5.5+. We use `generators <http://php.net/manual/en/language.generators.overview.php>`_
  liberally within the SDK code.
- We've upgraded the SDK to use `Guzzle 6 <http://guzzlephp.org>`_ (or 5), which
  provides the underlying HTTP client implementation used by the SDK to send
  requests to the AWS services. The latest version of Guzzle brings with it a
  number of improvements, including asynchronous requests, swappable HTTP
  handlers, PSR-7 compliance, better performance, and more.
- The PSR-7 package from the PHP-FIG (``psr/http-message``) defines interfaces
  for representing HTTP requests, HTTP responses, URLs, and streams. These
  interfaces are used across the SDK and Guzzle, which provides interoperability
  with other PSR-7 compliant packages.
- Guzzle's PSR-7 implementation (``guzzlehttp/psr7``) provides an implementation
  of the interfaces in PSR-7, and several helpful classes and
  functions. Both the SDK and Guzzle 6 rely on this package
  heavily.
- Guzzle's `Promises/A+ <https://promisesaplus.com>`_ implementation
  (``guzzlehttp/promises``) is used throughout the SDK and Guzzle to provide
  interfaces for managing asynchronous requests and coroutines. While Guzzle's
  multi-cURL HTTP handler ultimately implements the non-blocking I/O model that
  allows for asynchronous requests, this package provides the ability to program
  within that paradigm. See :doc:`guide_promises` for more details.
- The PHP implementation of `JMESPath <http://jmespath.org/>`_
  (``mtdowling/jmespath.php``) is used in the SDK to provide the data querying
  ability of the ``Aws\Result::search()`` and ``Aws\ResultPaginator::search()``
  methods. See :doc:`guide_jmespath` for more details.

Region and Version Options Are Now Required
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When instantiating a client for any service, you must specify the ``'region'``
and ``'version'`` options. In version 2 of the |sdk-php|, ``'version'`` was completely
optional, and ``'region'`` was sometimes optional. In version 3, both are always
required. Being explicit about both of these options allows you to lock into the
API version and AWS Region you are coding against. When new API versions are created
or new AWS Regions become available, you will be isolated from potentially breaking
changes until you are ready to explicitly update your configuration.

.. note::

    If you're not concerned about which API version you are using, you can
    just set the ``'version'`` option to ``'latest'``. However, we
    recommend that you set the API version numbers explicitly for production
    code.

    Not all services are available in all AWS Regions. You can find a list of
    available Regions using the :AWS-gr:`Regions and Endpoints <rande>` reference.

    For services that are available only via a single, global endpoint (e.g., |R53long|,
    |IAMlong|, and |CFLong|), you should instantiate clients with their configured
    Region set to ``us-east-1``.

.. important::

    The SDK also includes multi-region clients, which can dispatch requests to
    different AWS Regions based on a parameter (``@region``) supplied as a command
    parameter. The Region used by default by these clients is specified with the
    ``region`` option supplied to the client constructor.

Client Instantiation Uses the Constructor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In version 3 of the |sdk-php|, the way you instantiate a client has changed. Instead
of the ``factory`` methods in version 2, you can simply instantiate a client
by using the ``new`` keyword.

.. code-block:: php

    use Aws\DynamoDb\DynamoDbClient;

    // Version 2 style
    $client = DynamoDbClient::factory([
        'region'  => 'us-east-2'
    ]);

    // Version 3 style
    $client = new DynamoDbClient([
        'region'  => 'us-east-2',
        'version' => '2012-08-10'
    ]);

.. note::

    Instantiating a client using the ``factory()`` method still works. However, it's
    considered deprecated.

Client Configuration Has Changed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The client configuration options in version 3 of the |sdk-php| have changed a little
from version 2. See the :doc:`guide_configuration` page for a description of all
supported options.

.. important::

    In version 3, ``'key'`` and ``'secret'`` are no longer valid
    options at the root level, but you can pass them in as part of the
    ``'credentials'`` option. One reason we made this was to discourage
    developers from hard-coding their AWS credentials into their projects.

The Sdk Object
^^^^^^^^^^^^^^

Version 3 of the |sdk-php| introduces the ``Aws\Sdk`` object as a replacement to
``Aws\Common\Aws``. The ``Sdk`` object acts as a client factory and is used
to manage shared configuration options across multiple clients.

Although the ``Aws`` class in version 2 of the SDK worked like a service locator (it always
returned the same instance of a client), the ``Sdk`` class in version 3 returns a new
instance of a client every time it's used.

The ``Sdk`` object also doesn't support the same configuration file format from version 2 of
the SDK. That configuration format was specific to Guzzle 3 and is now obsolete.
Configuration can be done more simply with basic arrays, and is documented
in :ref:`sdk-class`.

Some API Results Have Changed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To provide consistency in how the SDK parses the result of an API
operation, |ELClong|, |RDS|, and |RSlong| now have an
additional wrapping element on some API responses.

For example, calling the |RDS| :RDS-api:`DescribeEngineDefaultParameters <API_DescribeEngineDefaultParameters>`
result in version 3 now includes a wrapping "EngineDefaults" element. In
version 2, this element was not present.

.. code-block:: php

    $client = new Aws\Rds\RdsClient([
        'region'  => 'us-west-1',
        'version' => '2014-09-01'
    ]);

    // Version 2
    $result = $client->describeEngineDefaultParameters();
    $family = $result['DBParameterGroupFamily'];
    $marker = $result['Marker'];

    // Version 3
    $result = $client->describeEngineDefaultParameters();
    $family = $result['EngineDefaults']['DBParameterGroupFamily'];
    $marker = $result['EngineDefaults']['Marker'];

The following operations are affected and now contain a wrapping element in the
output of the result (provided below in parentheses):

- |ELClong|

  - AuthorizeCacheSecurityGroupIngress (CacheSecurityGroup)
  - CopySnapshot (Snapshot)
  - CreateCacheCluster (CacheCluster)
  - CreateCacheParameterGroup (CacheParameterGroup)
  - CreateCacheSecurityGroup (CacheSecurityGroup)
  - CreateCacheSubnetGroup (CacheSubnetGroup)
  - CreateReplicationGroup (ReplicationGroup)
  - CreateSnapshot (Snapshot)
  - DeleteCacheCluster (CacheCluster)
  - DeleteReplicationGroup (ReplicationGroup)
  - DeleteSnapshot (Snapshot)
  - DescribeEngineDefaultParameters (EngineDefaults)
  - ModifyCacheCluster (CacheCluster)
  - ModifyCacheSubnetGroup (CacheSubnetGroup)
  - ModifyReplicationGroup (ReplicationGroup)
  - PurchaseReservedCacheNodesOffering (ReservedCacheNode)
  - RebootCacheCluster (CacheCluster)
  - RevokeCacheSecurityGroupIngress (CacheSecurityGroup)

- |RDS|

  - AddSourceIdentifierToSubscription (EventSubscription)
  - AuthorizeDBSecurityGroupIngress (DBSecurityGroup)
  - CopyDBParameterGroup (DBParameterGroup)
  - CopyDBSnapshot (DBSnapshot)
  - CopyOptionGroup (OptionGroup)
  - CreateDBInstance (DBInstance)
  - CreateDBInstanceReadReplica (DBInstance)
  - CreateDBParameterGroup (DBParameterGroup)
  - CreateDBSecurityGroup (DBSecurityGroup)
  - CreateDBSnapshot (DBSnapshot)
  - CreateDBSubnetGroup (DBSubnetGroup)
  - CreateEventSubscription (EventSubscription)
  - CreateOptionGroup (OptionGroup)
  - DeleteDBInstance (DBInstance)
  - DeleteDBSnapshot (DBSnapshot)
  - DeleteEventSubscription (EventSubscription)
  - DescribeEngineDefaultParameters (EngineDefaults)
  - ModifyDBInstance (DBInstance)
  - ModifyDBSubnetGroup (DBSubnetGroup)
  - ModifyEventSubscription (EventSubscription)
  - ModifyOptionGroup (OptionGroup)
  - PromoteReadReplica (DBInstance)
  - PurchaseReservedDBInstancesOffering (ReservedDBInstance)
  - RebootDBInstance (DBInstance)
  - RemoveSourceIdentifierFromSubscription (EventSubscription)
  - RestoreDBInstanceFromDBSnapshot (DBInstance)
  - RestoreDBInstanceToPointInTime (DBInstance)
  - RevokeDBSecurityGroupIngress (DBSecurityGroup)

- |RSlong|

  - AuthorizeClusterSecurityGroupIngress (ClusterSecurityGroup)
  - AuthorizeSnapshotAccess (Snapshot)
  - CopyClusterSnapshot (Snapshot)
  - CreateCluster (Cluster)
  - CreateClusterParameterGroup (ClusterParameterGroup)
  - CreateClusterSecurityGroup (ClusterSecurityGroup)
  - CreateClusterSnapshot (Snapshot)
  - CreateClusterSubnetGroup (ClusterSubnetGroup)
  - CreateEventSubscription (EventSubscription)
  - CreateHsmClientCertificate (HsmClientCertificate)
  - CreateHsmConfiguration (HsmConfiguration)
  - DeleteCluster (Cluster)
  - DeleteClusterSnapshot (Snapshot)
  - DescribeDefaultClusterParameters (DefaultClusterParameters)
  - DisableSnapshotCopy (Cluster)
  - EnableSnapshotCopy (Cluster)
  - ModifyCluster (Cluster)
  - ModifyClusterSubnetGroup (ClusterSubnetGroup)
  - ModifyEventSubscription (EventSubscription)
  - ModifySnapshotCopyRetentionPeriod (Cluster)
  - PurchaseReservedNodeOffering (ReservedNode)
  - RebootCluster (Cluster)
  - RestoreFromClusterSnapshot (Cluster)
  - RevokeClusterSecurityGroupIngress (ClusterSecurityGroup)
  - RevokeSnapshotAccess (Snapshot)
  - RotateEncryptionKey (Cluster)

Enum Classes Have Been Removed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We have removed the ``Enum`` classes (e.g., ``Aws\S3\Enum\CannedAcl``) that
existed in version 2 of the |sdk-php|. Enums were concrete classes within the public
API of the SDK that contained constants representing groups of valid parameter
values. Because these enums are specific to API versions, can change over time,
can conflict with PHP reserved words, and ended up not being very useful, we
have removed them in version 3. This supports the data-driven and API version
agnostic nature of version 3.

Instead of using values from ``Enum`` objects, you should use the literal
values directly (e.g., ``CannedAcl::PUBLIC_READ`` → ``'public-read'``).

Fine-Grained Exception Classes Have Been Removed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We have removed the fine-grained exception classes that existed in each
service's namespaces (e.g., ``Aws\Rds\Exception\{SpecificError}Exception``)
for very similar reasons that we removed Enums. The exceptions thrown by a
service or operation are dependent on which API version is used (they can
change from version to version). Also, the complete list of the exceptions that can
be thrown by a given operation is not available, which made version 2's
fine-grained exception classes incomplete.

You should handle errors by catching the root exception class for each service
(e.g., ``Aws\Rds\Exception\RdsException``). You can use the ``getAwsErrorCode()``
method of the exception to check for specific error codes. This is functionally
equivalent to catching different exception classes, but provides that function
without adding bloat to the SDK.

Static Facade Classes Have Been Removed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In version 2 of the |sdk-php|, there was an obscure feature inspired by Laravel that allowed you
to call ``enableFacades()`` on the ``Aws`` class to enable static access to the
various service clients. This feature goes against PHP best practices, and we
stopped documenting it over a year ago. In version 3, this feature is removed
completely. You should retrieve your client objects from the ``Aws\Sdk`` object
and use them as object instances, not static classes.

Paginators Supersede  iterators
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Version 2 of the |sdk-php| had a feature named * iterators*. These were objects that
were used for iterating over paginated results. One complaint we had about these
was that they were not flexible enough, because the iterator only emitted
specific values from each result. If there were other values you needed from
the results, you could only retrieve them via event listeners.

In version 3,  iterators have been replaced with :doc:`Paginators <guide_paginators>`.
Their purpose is similar, but paginators are more flexible. This is because they
yield result objects instead of values from a response.

The following examples show how paginators are different from  iterators,
by demonstrating how to retrieve paginated results for the ``S3 ListObjects`` operation
in both version 2 and version 3.

.. code-block:: php

    // Version 2
    $objects = $s3Client->getIterator('ListObjects', ['Bucket' => 'my-bucket']);
    foreach ($objects as $object) {
        echo $object['Key'] . "\n";
    }

.. code-block:: php

    // Version 3
    $results = $s3Client->getPaginator('ListObjects', ['Bucket' => 'my-bucket']);
    foreach ($results as $result) {
        // You can extract any data that you want from the result.
        foreach ($result['Contents'] as $object) {
            echo $object['Key'] . "\n";
        }
    }

Paginator objects have a ``search()`` method that enables you to use :doc:`JMESPath <guide_jmespath>`
expressions to extract data more easily from the result set.

.. code-block:: php

    $results = $s3Client->getPaginator('ListObjects', ['Bucket' => 'my-bucket']);
    foreach ($results->search('Contents[].Key') as $key) {
        echo $key . "\n";
    }

.. note::

    The ``getIterator()`` method is still supported to allow for a smooth
    transition to version 3, but we encourage you to migrate your code to use
    paginators.

Many Higher-Level Abstractions Have Changed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In general, many of the higher-level abstractions (service-specific helper
objects, aside from the clients) have been improved or updated. Some have
even been removed.

* Updated:
    * The way you use :doc:`Amazon S3 Multipart Upload <s3-multipart-upload>`
      has changed. |GLlong| Multipart Upload has been changed in similar ways.
    * The way to create :doc:`Amazon S3 pre-signed URLs <s3-presigned-url>` has changed.
    * The ``Aws\S3\Sync`` namespace has been replaced by the ``Aws\S3\Transfer``
      class. The ``S3Client::uploadDirectory()`` and ``S3Client::downloadBucket()``
      methods are still available, but have different options. See the documentation for
      :doc:`s3-examples-transfer`.
    * ``Aws\S3\Model\ClearBucket`` and ``Aws\S3\Model\DeleteObjectsBatch``
      have been replaced by ``Aws\S3\BatchDelete`` and ``S3Client::deleteMatchingObjects()``.
    * The options and behaviors for the :doc:`service_dynamodb-session-handler`
      have changed slightly.
    * The ``Aws\DynamoDb\Model\BatchRequest`` namespace has been replaced by
      ``Aws\DynamoDb\WriteRequestBatch``. See the documentation for
      :aws-php-class:`DynamoDB WriteRequestBatch </class-Aws.DynamoDb.WriteRequestBatch.html>`.

* Removed:
    * |DDBlong| ``Item``, ``Attribute``, and ``ItemIterator`` classes - These
      were previously deprecated in `Version 2.7.0 <https://github.com/aws/aws-sdk-php/blob/3.0.0/CHANGELOG.md#270---2014-10-08>`_.
    * |SNS| message validator - This is now `a separate, lightweight project
      <https://github.com/aws/aws-php-sns-message-validator>`_ that does not
      require the SDK as a dependency. This project is, however, included in the
      Phar and ZIP distributions of the SDK. You can find a getting started guide
      `on the AWS PHP Development blog <https://aws.amazon.com/blogs/developer/receiving-amazon-sns-messages-in-php/>`_.
    * |S3| ``AcpBuilder`` and related objects were removed.

Comparing Code Samples from Both Versions of the SDK
----------------------------------------------------

The following examples show some of the ways in which using version 3 of
the |sdk-php| might differ from version 2.

Example: |S3| ListObjects Operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

From Version 2 of the SDK
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    <?php

    require '/path/to/vendor/autoload.php';

    use Aws\S3\S3Client;
    use Aws\S3\Exception\S3Exception;

    $s3 = S3Client::factory([
        'profile' => 'my-credential-profile',
        'region'  => 'us-east-1'
    ]);

    try {
        $result = $s3->listObjects([
            'Bucket' => 'my-bucket-name',
            'Key'    => 'my-object-key'
        ]);

        foreach ($result['Contents'] as $object) {
            echo $object['Key'] . "\n";
        }
    } catch (S3Exception $e) {
        echo $e->getMessage() . "\n";
    }

From Version 3 of the SDK
^^^^^^^^^^^^^^^^^^^^^^^^^

Key differences:

- Use ``new`` instead of ``factory()`` to instantiate the client.
- The ``'version'`` and ``'region'`` options are required during instantiation.

.. code-block:: php

    <?php

    require '/path/to/vendor/autoload.php';

    use Aws\S3\S3Client;
    use Aws\S3\Exception\S3Exception;

    $s3 = new S3Client([
        'profile' => 'my-credential-profile',
        'region'  => 'us-east-1',
        'version' => '2006-03-01'
    ]);

    try {
        $result = $s3->listObjects([
            'Bucket' => 'my-bucket-name',
            'Key'    => 'my-object-key'
        ]);

        foreach ($result['Contents'] as $object) {
            echo $object['Key'] . "\n";
        }
    } catch (S3Exception $e) {
        echo $e->getMessage() . "\n";
    }

Example: Instantiating a Client with global Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

From Version 2 of the SDK
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: php

    <?php return array(
        'includes' => array('_aws'),
        'services' => array(
            'default_settings' => array(
                'params' => array(
                    'profile' => 'my_profile',
                    'region'  => 'us-east-1'
                )
            ),
            'dynamodb' => array(
                'extends' => 'dynamodb',
                'params' => array(
                    'region'  => 'us-west-2'
                )
            ),
        )
    );

.. code-block:: php

    <?php

    require '/path/to/vendor/autoload.php';

    use Aws\Common\Aws;

    $aws = Aws::factory('path/to/my/config.php');

    $sqs = $aws->get('sqs');
    // Note: SQS client will be configured for us-east-1.

    $dynamodb = $aws->get('dynamodb');
    // Note: DynamoDB client will be configured for us-west-2.

From Version 3 of the SDK
^^^^^^^^^^^^^^^^^^^^^^^^^

Key differences:

- Use the ``Aws\Sdk`` class instead of ``Aws\Common\Aws``.
- There's no configuration file. Use an array for configuration instead.
- The ``'version'`` option is required during instantiation.
- Use the ``create<Service>()`` methods instead of ``get('<service>')``.

.. code-block:: php

    <?php

    require '/path/to/vendor/autoload.php';

    $sdk = new Aws\Sdk([
        'profile' => 'my_profile',
        'region' => 'us-east-1',
        'version' => 'latest',
        'DynamoDb' => [
            'region' => 'us-west-2',
        ],
    ]);

    $sqs = $sdk->createSqs();
    // Note: Amazon SQS client will be configured for us-east-1.

    $dynamodb = $sdk->createDynamoDb();
    // Note: DynamoDB client will be configured for us-west-2.
