.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

==========================
Basic Use of the |sdk-php|
==========================

.. meta::
   :description: Describes the basic usage pattern of the AWS SDK for PHP.
   :keywords: AWS SDK for PHP, create a client, asynchronous requests, getting started with AWS SDK for PHP

This topic focuses on basic usage patterns of the |sdk-php|.

Prerequisites
-------------

-  :doc:`Download and installed the SDK <getting-started_installation>` 
-  Retrieve your `AWS access keys <http://aws.amazon.com/developers/access-keys/>`_.

Including the SDK in Your Code
------------------------------

No matter which technique you used to to install the SDK, you can include
the SDK in your code with just a single ``require`` statement. See
the following table for the PHP code that best fits your installation technique.
Replace any instances of ``/path/to/`` with the actual path on your system.

========================== =====================================================
Installation Technique     Require Statement
========================== =====================================================
Using Composer             ``require '/path/to/vendor/autoload.php';``
-------------------------- -----------------------------------------------------
Using the phar             ``require '/path/to/aws.phar';``
-------------------------- -----------------------------------------------------
Using the ZIP              ``require '/path/to/aws-autoloader.php';``
========================== =====================================================

In this topic, we show examples that assume the Composer
installation method. If you're using a different installation method, you
can refer back to this section to find the correct ``require`` code to use.

Usage Summary
-------------

To use the SDK to interact with an AWS service, instantiate a **Client** object. 
Client objects have methods that
correspond one to one with operations in the service's API. To execute a
particular operation, you call its corresponding method. This method either returns an
array-like **Result** object on success, or throws an **Exception** on failure.

Creating a Client
-----------------

You can create a client by passing an associative array of options to a
client's constructor.

**Imports**

.. literalinclude::  example_code/s3/CreateClient.php
   :lines: 16-18
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/CreateClient.php
   :lines: 28-32
   :language: php

Notice that we did **not** explicitly provide credentials to the client. That's
because the SDK should detect the credentials from
:ref:`environment variables <environment_credentials>` (via
``AWS_ACCESS_KEY_ID`` and ``AWS_SECRET_ACCESS_KEY``), an
:ref:`AWS credentials INI file <credential_profiles>` in your HOME
directory, |IAMlong| (IAM)
:ref:`instance profile credentials <instance_profile_credentials>`, or
:ref:`credential providers <credential_provider>`.

All of the general client configuration options are described in detail in
the :doc:`configuration guide <guide_configuration>`. The array of options
provided to a client can vary based on which client you're creating. These
custom client configuration options are described in the
`API documentation <http://docs.aws.amazon.com/aws-sdk-php/latest/>`_ for each
client.

.. _sdk-class:

Using the Sdk Class
-------------------

The ``Aws\Sdk`` class acts as a client factory and is used to manage shared
configuration options across multiple clients. The same options that can be
provided to a specific client constructor can also be supplied to the
``Aws\Sdk`` class. These options are then applied to each client constructor.

**Imports**

.. literalinclude::  example_code/s3/CreateClient.php
   :lines: 16-18
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/CreateClient.php
   :lines: 35-45
   :language: php

Options that are shared across all clients are placed in root-level key-value
pairs. Service-specific configuration data can be provided in a key that is the
same as the namespace of a service (e.g., "S3", "DynamoDb", etc.).

.. code-block:: php

    $sdk = new Aws\Sdk([
        'region'   => 'us-west-2',
        'version'  => 'latest',
        'DynamoDb' => [
            'region' => 'eu-central-1'
        ]
    ]);

    // Creating an Amazon DynamoDb client will use the "eu-central-1" AWS Region
    $client = $sdk->createDynamoDb();

Service-specific configuration values are a union of the service-specific
values and the root-level values (i.e., service-specific values are
shallow-merged onto root-level values).

.. tip::

    We highly recommended that you use the ``Sdk`` class to create clients
    if you're using multiple client instances in your application. The
    ``Sdk`` class automatically uses the same HTTP client for each SDK
    client, allowing SDK clients for different services to perform nonblocking
    HTTP requests. If the SDK clients don't use the same HTTP client, then
    HTTP requests sent by the SDK client might block promise orchestration between services.

Executing Service Operations
----------------------------

You can execute a service operation by calling the method of the same name on
a client object. For example, to perform the |S3|  :S3-api:`PutObject operation
<RESTObjectPUT>`, you must
call the ``Aws\S3\S3Client::putObject()`` method.

**Imports**

.. literalinclude::  example_code/s3/PutObjectServiceOperations.php
   :lines: 16-18
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/PutObjectServiceOperations.php
   :lines: 27-53
   :language: php

Operations available to a client and the structure of the input and output are
defined at runtime based on a service description file. When creating a client,
you must provide a version (e.g., `"2006-03-01"` or `"latest"`). The SDK
finds the corresponding configuration file based on the provided version.

Operation methods like ``putObject()`` all accept a single argument, an
associative array that represents the parameters of the operation. The structure
of this array (and the structure of the result object) is defined for each
operation in the SDK's API Documentation (e.g., see the API docs for
:aws-php-class:`putObject operation </api-s3-2006-03-01.html#putobject>`).

HTTP Handler Options
~~~~~~~~~~~~~~~~~~~~

You can also fine-tune how the underlying HTTP handler executes the
request by using the special ``@http`` parameter. The options you can include
in the ``@http`` parameter are the same as the ones you can set when you
instantiate the client with the :ref:`"http" client option <config_http>`.

.. code-block:: php

    // Send the request through a proxy
    $result = $s3Client->putObject([
        'Bucket' => 'my-bucket',
        'Key'    => 'my-key',
        'Body'   => 'this is the body!',
        '@http'  => [
            'proxy' => 'http://192.168.16.1:10'
        ]
    ]);

Asynchronous Requests
---------------------

You can send commands concurrently using the asynchronous features of the SDK.
You can send requests asynchronously by suffixing an operation name with
``Async``. This initiates the request and returns a promise. The promise
is fulfilled with the result object on success or rejected with an
exception on failure. This enables you to create multiple promises and
have them send HTTP requests concurrently when the underlying HTTP handler
transfers the requests.

**Imports**

.. literalinclude::  example_code/s3/ListBucketsAsync.php
   :lines: 16-18
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/ListBucketsAsync.php
   :lines: 27-39
   :language: php

You can force a promise to complete synchronously by using the ``wait`` method of
the promise. Forcing the promise to complete also "unwraps" the state of
the promise by default, meaning it will either return the result of the promise
or throw the exception that was encountered. When calling ``wait()`` on a
promise, the process blocks until the HTTP request is completed and the
result is populated or an exception is thrown.

When using the SDK with an event loop library, don't block on
results. Instead, use the ``then()`` method of a result to access a promise
that is resolved or rejected when the operation completes.

**Imports**

.. literalinclude::  example_code/s3/ListBucketsAsync.php
   :lines: 16-18
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/ListBucketsAsync.php
   :lines: 27-35
   :language: php

.. literalinclude:: example_code/s3/ListBucketsAsync.php
   :lines: 41-49
   :language: php


.. _result_objects:

Working with Result Objects
---------------------------

Executing a successful operation returns an ``Aws\Result`` object. Instead
of returning the raw XML or JSON data of a service, the SDK coerces the response
data into an associative array structure. It normalizes some aspects of the data
based on its knowledge of the specific service and the underlying response
structure.

You can access data from the AWS\Result object like an associative PHP array.

**Imports**

.. literalinclude::  example_code/s3/ListBucketsResultObject.php
   :lines: 16-18
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/ListBucketsResultObject.php
   :lines: 27-45
   :language: php



The contents of the result object depend on the operation that was executed
and the version of a service. The result structure of each API operation is
documented in the API docs for each operation.

The SDK is integrated with `JMESPath <http://jmespath.org/>`_, a `DSL
<http://en.wikipedia.org/wiki/Domain-specific_language>`_ used to search and
manipulate JSON data or, in our case, PHP arrays. The result object contains a
``search()`` method you can use to more declaratively extract data from the
result.

**Code**

.. literalinclude:: example_code/s3/ListBucketsResultObject.php
   :lines: 37-39
   :language: php

.. literalinclude:: example_code/s3/ListBucketsResultObject.php
  :lines: 47-48
  :language: php



Handling Errors
---------------

Synchronous Error Handling
~~~~~~~~~~~~~~~~~~~~~~~~~~

If an error occurs while performing an operation, an exception is thrown.
For this reason, if you need to handle errors in your code, you should use
``try``/``catch`` blocks around your operations. The SDK throws service-specific
exceptions when an error occurs.

The following example uses the ``Aws\S3\S3Client``. If there is an
error, the exception thrown will be of the type ``Aws\S3\Exception\S3Exception``.
All service-specific exceptions that the SDK throws extend from the
``Aws\Exception\AwsException`` class. This class contains useful information
about the failure, including the request-id, error code, and error type.


**Imports**

.. literalinclude::  example_code/s3/ErrorHandling.php
   :lines: 16-20
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/ErrorHandling.php
   :lines: 27-45
   :language: php


Asynchronous Error Handling
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Exceptions are not thrown when sending asynchronous requests. Instead, you must
use the ``then()`` or ``otherwise()`` method of the returned promise to
receive the result or error.

**Imports**

.. literalinclude::  example_code/s3/ErrorHandling.php
   :lines: 16-20
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/ErrorHandling.php
   :lines: 52-61
   :language: php


You can "unwrap" the promise and cause the exception to be thrown instead.

**Imports**

.. literalinclude::  example_code/s3/ErrorHandling.php
   :lines: 16-20
   :language: PHP

**Code**

.. literalinclude:: example_code/s3/ErrorHandling.php
   :lines: 52
   :language: php

.. literalinclude:: example_code/s3/ErrorHandling.php
  :lines: 64-68
  :language: php
