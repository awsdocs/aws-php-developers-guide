.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

############################################
|S3| Pre-Signed URL with |sdk-php| Version 3
############################################

.. meta::
   :description: Create direct browser access to private Amazon S3 data using the AWS SDK for PHP version 3.
   :keywords: Amazon S3 , AWS SDK for PHP version 3 examples, Amazon S3 for PHP code examples

You can authenticate certain types of requests by passing the required
information as query-string parameters instead of using the Authorization HTTP
header. This is useful for enabling direct third-party browser access to your
private |S3| data, without proxying the request. The idea is to construct
a "pre-signed" request and encode it as a URL that an end-user's browser can
retrieve. Additionally, you can limit a pre-signed request by specifying an
expiration time.

Creating a Pre-Signed Request
=============================

You can get the pre-signed URL to an |S3| object by using the
``Aws\S3\S3Client::createPresignedRequest()`` method. This method accepts an
``Aws\CommandInterface`` object and expired timestamp and returns a pre-signed
``Psr\Http\Message\RequestInterface`` object. You can retrieve the pre-signed
URL of the object using the ``getUri()`` method of the request.

The most common scenario is creating a pre-signed URL to GET an object.

**Sample Code**

.. literalinclude:: example_code/s3/PresignedURL.php
   :lines: 25-37
   :language: php

Creating a Pre-Signed URL
=========================

You can create pre-signed URLs for any |S3| operation using the
``getCommand`` method for creating a command object, and then calling the
``createPresignedRequest()`` method with the command. When ultimately sending
the request, be sure to use the same method and the same headers as the
returned request.

**Sample Code**

.. literalinclude:: example_code/s3/PresignedURL.php
   :lines: 39-48
   :language: php

Getting the URL to an Object
============================

If you only need the public URL to an object stored in an |S3| bucket,
you can use the ``Aws\S3\S3Client::getObjectUrl()`` method. This method
returns an unsigned URL to the given bucket and key.

**Sample Code**

.. literalinclude:: example_code/s3/PresignedURL.php
   :lines: 49-
   :language: php

.. important::

    The URL returned by this method is not validated to ensure that the bucket
    or key exists, nor does this method ensure that the object allows
    unauthenticated access.
