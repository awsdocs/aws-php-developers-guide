.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################
Paginators in the |sdk-php| Version 3
#######################################

.. meta::
   :description: Processing paginated results from the AWS SDK for PHP version 3.
   :keywords: AWS SDK for PHP version 3, paginators, iterate through results, PHP for AWS


Some AWS service operations are paginated and respond with truncated results.
For example, the |S3| ``ListObjects`` operation only returns up to 1,000
objects at a time. Operations like these (typically prefixed with "list" or
"describe") require making subsequent requests with token (or marker) parameters
to retrieve the entire set of results.

**Paginators** are a feature of the |sdk-php| that act as an abstraction over
this process to make it easier for developers to use paginated APIs. A paginator
is essentially an iterator of results. They are created via the
``getPaginator()`` method of the client. When you call ``getPaginator()``, you
must provide the name of the operation and the operation's arguments (in the
same way you do when you execute an operation). You can iterate over a paginator
object using ``foreach`` to get individual ``Aws\Result`` objects.

.. code-block:: php

    $results = $s3Client->getPaginator('ListObjects', [
        'Bucket' => 'my-bucket'
    ]);

    foreach ($results as $result) {
        foreach ($result['Contents'] as $object) {
            echo $object['Key'] . "\n";
        }
    }

Paginator Objects
=================

The object returned by ``getPaginator()`` method is an instance of the
``Aws\ResultPaginator`` class. This class implements PHP's native ``iterator``
interface, which is why it works with ``foreach``. It can also be used with
iterator functions, like ``iterator_to_array``, and integrates well with
`SPL iterators <http://www.php.net/manual/en/spl.iterators.php>`_ like the
``LimitIterator`` object.

Paginator objects hold only one "page" of results at a time and are executed
lazily. This means that they make only as many requests as they need to yield
the current page of results. For example, the |S3| ``ListObjects`` operation only
returns up to 1,000 objects at a time, so if your bucket has ~10,000 objects,
the paginator would need to do 10 requests total. When you iterate through the
results, the first request is executed when you start iterating, the second in
the second iteration of the loop, and so on.

Enumerating Data from Results
=============================

Paginator objects have a method named ``search()``, which allows you to create
iterators for data within a set of results. When you call ``search()``, you must
provide a :doc:`JMESPath expression <guide_jmespath>` to specify what data to extract.
Calling ``search()`` returns an iterator that yields the results of the
expression on each page of results. This is evaluated lazily, as you iterate
through the returned iterator.

The following example is equivalent to the preceding code example, but uses the
``ResultPaginator::search()`` method to be more concise.

.. code-block:: php

    $results = $s3Client->getPaginator('ListObjects', [
        'Bucket' => 'my-bucket'
    ]);

    foreach ($results->search('Contents[].Key') as $key) {
        echo $key . "\n";
    }

JMESPath expressions enable you to do fairly complex things. For example, if you
wanted to print all of the object keys and common prefixes (i.e., do an ``ls``
of a bucket), you could do the following.

.. code-block:: php

    // List all prefixes ("directories") and objects ("files") in the bucket
    $results = $s3Client->getPaginator('ListObjects', [
        'Bucket'    => 'my-bucket',
        'Delimiter' => '/'
    ]);

    $expression = '[CommonPrefixes[].Prefix, Contents[].Key][]';
    foreach ($results->search($expression) as $item) {
        echo $item . "\n";
    }

.. _async_paginators:

Asynchronous Pagination
=======================

You can iterate over the results of a paginator asynchronously by providing a
callback for the ``each()`` method of an ``Aws\ResultPaginator``. The callback
is invoked for each value that is yielded by the paginator.

.. code-block:: php

    $results = $s3Client->getPaginator('ListObjects', [
        'Bucket' => 'my-bucket'
    ]);

    $promise = $results->each(function ($result) {
        echo 'Got ' . var_export($result, true) . "\n";
    });

.. tip::

    Using the ``each()`` method allows you to paginate over the results of an
    API operation while concurrently sending other requests asynchronously.

A non-null return value from the callback will be yielded by the underlying
coroutine-based promise. This means that you can return promises from the
callback that must be resolved before continuing iteration over the remaining
items, essentially merging in other promises to the iteration. The last
non-null value returned by the callback is the result that fulfills the
promise to any downstream promises. If the last return value is a promise,
the resolution of that promise is the result that fulfills or rejects
downstream promises.

.. code-block:: php

    // Delete all keys that end with "Foo"
    $promise = $results->each(function ($result) use ($s3Client) {
        if (substr($result['Key'], -3) ### 'Foo') {
            // Merge this promise into the iterator
            return $s3Client->deleteAsync([
                'Bucket' => 'my-bucket',
                'Key'    => 'Foo'
            ]);
        }
    });

    $promise
        ->then(function ($result) {
            // Result would be the last result to the deleteAsync operation
        })
        ->otherwise($reason) {
            // Reason would be an exception that was encountered either in the
            // call to deleteAsync or calls performed while iterating
        });

    // Forcing a synchronous wait will also wait on all of the deleteAsync calls
    $promise->wait();
