.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.
   
########
Glossary
########

.. meta::
   :description:  Learn the frequently used terms to utilize the AWS SDK for PHP version 3. 
   :keywords: AWS SDK for PHP version 3, php for aws

API Version
    Services have one or more API versions, and which version you are using
    dictates which operations and parameters are valid. API versions are
    formatted like a date. For example, the latest API version for |S3| is
    ``2006-03-01``. You must :ref:`specify a version <cfg_version>` when you
    configure a client object.

Client
    Client objects are used to execute operations for a service. Each service
    that is supported in the SDK has a corresponding client object. Client
    objects have methods that correspond one-to-one with the service operations.
    See the :doc:`basic usage guide <getting-started_basic-usage>` for details
    on how to create and use client objects.

Command
    Command objects encapsulate the execution of an operation. When following
    the :doc:`basic usage patterns <getting-started_basic-usage>` of the SDK,
    you will not deal directly with command objects. Command objects can be
    accessed using the ``getCommand()`` method of a client, in order to use
    advanced features of the SDK like concurrent requests and batching. See
    the :doc:`guide_commands` guide for more details.

Credentials
    To interact with AWS services, you must authenticate with the service using
    your credentials, or `AWS access keys
    <http://aws.amazon.com/developers/access-keys/>`_. Your access keys consist
    of two parts: your access key ID, which identifies your account, and your
    secret access, which is used to create **signatures** when executing
    operations. You must :doc:`provide credentials <guide_credentials>` when
    you configure a client object.

Handler
    A handler is a function that performs the actual transformation of a command
    and request into a result. A handler typically sends HTTP requests. Handlers
    can be composed with middleware to augment their behavior. A handler is a
    function that accepts an ``Aws\CommandInterface`` and a
    ``Psr\Http\Message\RequestInterface`` and returns a promise that is fulfilled
    with an ``Aws\ResultInterface`` or rejected with an
    ``Aws\Exception\AwsException`` reason.


JMESPath
    `JMESPath <http://jmespath.org/>`_ is a query language for JSON-like data.
    The |sdk-php| uses JMESPath expressions to query PHP data structures.
    JMESPath expressions can be used directly on ``Aws\Result`` and
    ``Aws\ResultPaginator`` objects via the ``search($expression)`` method.

Middleware
    Middleware are a special type of high level function that that augment the
    behavior of transferring a command and delegate to a "next" handler. Middleware
    functions accept an ``Aws\CommandInterface`` and a
    ``Psr\Http\Message\RequestInterface`` and return a promise that is fulfilled
    with an ``Aws\ResultInterface`` or rejected with an
    ``Aws\Exception\AwsException`` reason.


Operation
    Refers to a single operation within a service's API (e.g., ``CreateTable``
    for |DDB|, ``RunInstances`` for |EC2|). In the SDK, operations are
    executed by calling a method of the same name on the corresponding service's
    client object. Executing an operation involves preparing and sending an HTTP
    request to the service and parsing the response. This process of executing
    an operation is abstracted by the SDK via **command** objects.

Paginator
    Some AWS service operations are paginated and respond with truncated
    results. For example, |S3|'s ``ListObjects`` operation only returns up
    to 1000 objects at a time. Operations like these require making subsequent
    requests with token (or marker) parameters to retrieve the entire set of
    results. paginators are a feature of the SDK that act as an abstraction over
    this process to make it easier for developers to use paginated APIs. They
    are accessed via the ``getPaginator()`` method of the client. See the
    :doc:`guide_paginators` guide for more details.

Promise
    A promise represents the eventual result of an asynchronous operation. The
    primary way of interacting with a promise is through its then method, which
    registers callbacks to receive either a promise's eventual value or the
    reason why the promise cannot be fulfilled.

Region
    Services are supported in :AWS-gr:`one or more geographical regions <rande>`. 
    Services may have different endpoints/URLs in each region, which exist to reduce data
    latency in your applications. You must :ref:`provide a region <cfg_region>`
    when you configure a client object, so that the SDK can determine which
    endpoint to use with the service.

SDK
    The term "SDK" can refer to the |sdk-php| library as a whole, but also
    refers to the ``Aws\Sdk`` class :aws-php-class:`(docs)
    </class-Aws.Sdk.html>`, which
    acts as a factory for the client objects for each **service**. The ``Sdk``
    class also let's you provide a set of :doc:`global configuration values
    <guide_configuration>` that are applied to all client objects that it
    creates.

Service
    A general way to refer to any of the AWS services (e.g., |S3|, |DDBlong|,
    AWS OpsWorks, etc.). Each service has a corresponding **client**
    object in the SDK that supports one or more **API versions**. Each service
    also has one or more **operations** that make up its API. Services are
    supported in one or more **regions**.

Signature
    When executing operations, the SDK uses your credentials to create a digital
    signature of your request. The service then verifies the signature before
    processing your request. The signing process is encapsulated by the SDK, and
    happens automatically using the credentials you configure for the client.

Waiter
    Waiters are a feature of the SDK that make it easier to work with operations
    that change the state of a resource and that are *eventually consistent* or
    *asynchronous* in nature. For example, the |DDBlong| ``CreateTable``
    operation sends a response back immediately, but the table may not be ready
    to access for several seconds. Executing a waiter allows you to wait until a
    resource enters into a particular state by sleeping and polling the
    resource's status. Waiters are accessed using the ``waitUntil()`` method of
    the client. See the :doc:`guide_waiters` guide for more details.
	
For the latest AWS terminology, see the :AWS-gr:`AWS Glossary <glos-chap>` in the AWS General Reference.
