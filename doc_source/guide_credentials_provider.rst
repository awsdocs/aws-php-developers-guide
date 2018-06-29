.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###########################
Using a Credential Provider
###########################

.. meta::
   :description: How to configure anonymous access for AWS Services using the AWS SDK for PHP.
   :keywords:

.. _credential_provider:

A credential provider is a function that returns a ``GuzzleHttp\Promise\PromiseInterface``
that is fulfilled with an ``Aws\Credentials\CredentialsInterface`` instance or
rejected with an ``Aws\Exception\CredentialsException``. You can use credential
providers to implement your own custom logic for creating credentials or to
optimize credential loading.

Credential providers are passed into the ``credentials`` client constructor
option. Credential providers are asynchronous, which forces them to be lazily
evaluated each time an API operation is invoked. As such, passing in a
credential provider function to an SDK client constructor doesn't immediately
validate the credentials. If the credential provider doesn't return a
credentials object, an API operation will be rejected with an
``Aws\Exception\CredentialsException``.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\S3\S3Client;

    // Use the default credential provider
    $provider = CredentialProvider::defaultProvider();

    // Pass the provider to the client
    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => '2006-03-01',
        'credentials' => $provider
    ]);
    
Built-In Providers in the SDK
=============================

The SDK provides several built-in providers that you can combine
with any custom providers.

.. important::

    Credential providers are invoked every time an API operation is performed.
    If loading credentials is an expensive task (e.g., loading from disk or a
    network resource), or if credentials are not cached by your provider,
    you should consider wrapping your credential provider in an
    ``Aws\Credentials\CredentialProvider::memoize`` function. The default
    credential provider used by the SDK is automatically memoized.

    
assumeRole provider
-------------------

If you use ``Aws\Credentials\AssumeRoleCredentialProvider`` to create credentials by assuming a role,
you need to provide ``'client'`` information with an ``StsClient`` object and
``'assume_role_params'`` details, as shown.

.. note::

   To avoid unnecessarily fetching |STS| credentials on every API operation, you can use
   the ``memoize`` function to handle automatically refreshing the credentials when they expire.
   See the following code for an example.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\S3\S3Client;
    use Aws\Sts\StsClient;

    // Passing Aws\Credentials\AssumeRoleCredentialProvider options directly
    $assumeRoleCredentials = new AssumeRoleCredentialProvider([
        'client' => new StsClient([
            'region' => 'us-west-2',
            'version' => '2011-06-15'
        ]),
        'assume_role_params' => [
            'RoleArn' => 'arn:aws:iam::123456789012:role/role_name',
            'RoleSessionName' => 'test_session',
        ]
    ]);

    // To avoid unnecessarily fetching STS credentials on every API operation,
    // the memoize function handles automatically refreshing the credentials when they expire
    $provider = CredentialProvider::memoize($provider);

    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => 'latest',
        'credentials' => $provider
    ]);


For more information regarding ``'assume_role_params'``, see :aws-php-class:`AssumeRole </api-sts-2011-06-15.html#assumerole>`.

Chaining Providers
------------------

You can chain credential providers by using the
``Aws\Credentials\CredentialProvider::chain()`` function. This function accepts
a variadic number of arguments, each of which are credential provider
functions. This function then returns a new function that's the composition of
the provided functions, such that they are invoked one after the other until one
of the providers returns a promise that is fulfilled successfully.

The ``defaultProvider`` uses this composition to check multiple
providers before failing. The source of the ``defaultProvider`` demonstrates
the use of the ``chain`` function.

.. code-block:: php

    // This function returns a provider
    public static function defaultProvider(array $config = [])
    {
        // This function is the provider, which is actually the composition
        // of multiple providers. Notice that we are also memoizing the result by
        // default.
        return self::memoize(
            self::chain(
                self::env(),
                self::ini(),
                self::instanceProfile($config)
            )
        );
    }

Creating a Custom Provider
--------------------------

Credential providers are simply functions that when invoked return a promise
(``GuzzleHttp\Promise\PromiseInterface``) that is fulfilled with an
``Aws\Credentials\CredentialsInterface`` object or rejected with an
``Aws\Exception\CredentialsException``.

A best practice for creating providers is to create a function that is invoked
to create the actual credential provider. As an example, here's the source of
the ``env`` provider (slightly modified for example purposes). Notice that it
is a function that returns the actual provider function. This allows you to
easily compose credential providers and pass them around as values.

.. code-block:: php

    use GuzzleHttp\Promise;
    use GuzzleHttp\Promise\RejectedPromise;

    // This function CREATES a credential provider
    public static function env()
    {
        // This function IS the credential provider
        return function () {
            // Use credentials from environment variables, if available
            $key = getenv(self::ENV_KEY);
            $secret = getenv(self::ENV_SECRET);
            if ($key && $secret) {
                return Promise\promise_for(
                    new Credentials($key, $secret, getenv(self::ENV_SESSION))
                );
            }

            $msg = 'Could not find environment variable '
                . 'credentials in ' . self::ENV_KEY . '/' . self::ENV_SECRET;
            return new RejectedPromise(new CredentialsException($msg));
        };
    }
    
    
defaultProvider provider
------------------------

``Aws\Credentials\CredentialProvider::defaultProvider`` is the default
credential provider. This provider is used if you omit a ``credentials`` option
when creating a client. It first attempts to load credentials from environment
variables, then from an .ini file (an ``.aws/credentials`` file first, followed by an ``.aws/config`` file),
and then from an instance profile (``EcsCredentials`` first, followed by ``Ec2`` metadata).

.. note::

    The result of the default provider is automatically memoized.

ecsCredentials provider
-----------------------

``Aws\Credentials\CredentialProvider::ecsCredentials`` attempts to load
credentials by a ``GET`` request, whose URI is specified by the environment variable
``AWS_CONTAINER_CREDENTIALS_RELATIVE_URI`` in the container.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\S3\S3Client;

    $provider = CredentialProvider::ecsCredentials();
    // Be sure to memoize the credentials
    $memoizedProvider = CredentialProvider::memoize($provider);

    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => '2006-03-01',
        'credentials' => $memoizedProvider
    ]);


env provider
------------

``Aws\Credentials\CredentialProvider::env`` attempts to load credentials from
environment variables.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\S3\S3Client;

    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => '2006-03-01',
        'credentials' => CredentialProvider::env()
    ]);

ini provider
------------

``Aws\Credentials\CredentialProvider::ini`` attempts to load credentials from
an :doc:`ini credential file <guide_credentials_profiles>`. By default, the SDK
attempts to load the "default" profile from a file located at
``~/.aws/credentials``.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\S3\S3Client;

    $provider = CredentialProvider::ini();
    // Cache the results in a memoize function to avoid loading and parsing
    // the ini file on every API operation
    $provider = CredentialProvider::memoize($provider);

    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => '2006-03-01',
        'credentials' => $provider
    ]);

You can use a custom profile or .ini file location by providing arguments to
the function that creates the provider.

.. code-block:: php

    $profile = 'production';
    $path = '/full/path/to/credentials.ini';

    $provider = CredentialProvider::ini($profile, $path);
    $provider = CredentialProvider::memoize($provider);

    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => '2006-03-01',
        'credentials' => $provider
    ]);

instanceProfile provider
------------------------

``Aws\Credentials\CredentialProvider::instanceProfile`` attempts to load
credentials from |EC2| instance profiles.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\S3\S3Client;

    $provider = CredentialProvider::instanceProfile();
    // Be sure to memoize the credentials
    $memoizedProvider = CredentialProvider::memoize($provider);

    $client = new S3Client([
        'region'      => 'us-west-2',
        'version'     => '2006-03-01',
        'credentials' => $memoizedProvider
    ]);

.. note::

    You can disable this attempt to load from |EC2| instance profiles by
    setting the ``AWS_EC2_METADATA_DISABLED`` environment variable to ``true``.

Memoizing Credentials
---------------------

At times you might need to create a credential provider that remembers the
previous return value. This can be useful for performance when loading
credentials is an expensive operation or when using the ``Aws\Sdk`` class to
share a credential provider across multiple clients. You can add memoization to
a credential provider by wrapping the credential provider function in a
``memoize`` function.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;

    $provider = CredentialProvider::instanceProfile();
    // Wrap the actual provider in a memoize function
    $provider = CredentialProvider::memoize($provider);

    // Pass the provider into the Sdk class and share the provider
    // across multiple clients. Each time a new client is constructed,
    // it will use the previously returned credentials as long as
    // they haven't yet expired.
    $sdk = new Aws\Sdk(['credentials' => $provider]);

    $s3 = $sdk->getS3(['region' => 'us-west-2', 'version' => 'latest']);
    $ec2 = $sdk->getEc2(['region' => 'us-west-2', 'version' => 'latest']);

    assert($s3->getCredentials() === $ec2->getCredentials());

When the memoized credentials are expired, the memoize wrapper invokes
the wrapped provider in an attempt to refresh the credentials.
