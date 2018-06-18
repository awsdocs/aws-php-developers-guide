.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###########################################################
Using |S3| Multipart Uploads with AWS SDK for PHP version 3 
###########################################################

.. meta::
   :description: Break larger files into smaller parts when you upload to Amazon S3 using the AWS SDK for PHP version 3 .
   :keywords: Amazon S3, AWS SDK for PHP version 3  examples, |S3| for PHP code examples


With a single ``PutObject`` operation, you can upload objects up to 5 GB in
size. However, by using the multipart upload methods (e.g., ``CreateMultipartUpload``,
``UploadPart``, ``CompleteMultipartUpload``, ``AbortMultipartUpload``), you can
upload objects from 5 MB to 5 TB in size.

Multipart uploads are designed to improve the upload experience for larger
objects. They enable you to upload objects in parts
independently, in any order, and in parallel.

|S3| customers are encouraged to use multipart uploads for objects greater
than 100 MB.

MultipartUploader Object
========================

The SDK has a special ``MultipartUploader`` object that simplifies the multipart upload
process.

.. code-block:: php

    use Aws\S3\MultipartUploader;
    use Aws\Exception\MultipartUploadException;

    $uploader = new MultipartUploader($s3Client, '/path/to/large/file.zip', [
        'bucket' => 'your-bucket',
        'key'    => 'my-file.zip',
    ]);

    try {
        $result = $uploader->upload();
        echo "Upload complete: {$result['ObjectURL']}\n";
    } catch (MultipartUploadException $e) {
        echo $e->getMessage() . "\n";
    }

The uploader creates a generator of part data, based on the provided source and
configuration, and attempts to upload all parts. If some part uploads fail, the
uploader keeps track of them and continues to upload later parts until the entire
source data is read. It then either completes the upload or throws an
exception that contains information about the parts that failed to upload.

Customizing a Multipart Upload
==============================

You can set custom options on the ``CreateMultipartUpload``, ``UploadPart``, and
``CompleteMultipartUpload`` operations executed by the multipart uploader via
callbacks passed to its constructor.

.. code-block:: php

    $source = '/path/to/large/file.zip';
    $uploader = new MultipartUploader($s3Client, $source, [
        'bucket' => 'your-bucket',
        'key'    => 'my-file.zip',
        'before_initiate' => function (\Aws\Command $command) {
            // $command is a CreateMultipartUpload operation
            $command['CacheControl'] = 'max-age=3600';
        },
        'before_upload' => function (\Aws\Command $command) {
           // $command is an UploadPart operation
           $command['RequestPayer'] = 'requester';
        },
        'before_complete' => function (\Aws\Command $command) {
           // $command is a CompleteMultipartUpload operation
           $command['RequestPayer'] = 'requester';
        },
    ]);

Recovering from Errors
======================

When an error occurs during the multipart upload process, a
``MultipartUploadException`` is thrown. This exception provides access to the
``UploadState`` object, which contains information about the multipart upload's
progress. The ``UploadState`` can be used to resume an upload that failed to
complete.

.. code-block:: php

    $source = '/path/to/large/file.zip';
    $uploader = new MultipartUploader($s3Client, $source, [
        'bucket' => 'your-bucket',
        'key'    => 'my-file.zip',
    ]);

    do {
        try {
            $result = $uploader->upload();
        } catch (MultipartUploadException $e) {
            $uploader = new MultipartUploader($s3Client, $source, [
                'state' => $e->getState(),
            ]);
        }
    } while (!isset($result));

Resuming an upload from an ``UploadState`` will only attempt to upload parts
that are not already uploaded. The state object keeps track of missing parts,
even if they are not consecutive. The uploader reads or seeks through the
provided source file to the byte ranges that belong to the parts that still need
to be uploaded.

``UploadState`` objects are serializable, so you can also resume an
upload in a different process. You can also get the ``UploadState`` object, even
when you're not handling an exception, by calling ``$uploader->getState()``.

.. important::

    Streams passed in as a source to a ``MultipartUploader`` are not
    automatically rewound before uploading. If you're using a stream instead of a
    file path in a loop similar to the previous example, you need to reset the
    ``$source`` variable inside of the ``catch`` block.

    .. code-block:: php

        $source = fopen('/path/to/large/file.zip', 'rb');
        $uploader = new MultipartUploader($s3Client, $source, [
            'bucket' => 'your-bucket',
            'key'    => 'my-file.zip',
        ]);

        do {
            try {
                $result = $uploader->upload();
            } catch (MultipartUploadException $e) {
                rewind($source);
                $uploader = new MultipartUploader($s3Client, $source, [
                    'state' => $e->getState(),
                ]);
            }
        } while (!isset($result));

Aborting a Multipart Upload
---------------------------

Sometimes, you might not want to resume an upload, and would rather
abort the the whole thing when an error occurs. This is also easy using the
data contained in the ``UploadState`` object.

.. code-block:: php

    try {
        $result = $uploader->upload();
    } catch (MultipartUploadException $e) {
        // State contains the "Bucket", "Key", and "UploadId"
        $params = $e->getState()->getId();
        $result = $s3Client->abortMultipartUpload($params);
    }

Asynchronous Multipart Uploads
==============================

Calling ``upload()`` on the ``MultipartUploader`` is a blocking request. If you are
working in an asynchronous context, you can get a :doc:`promise <guide_promises>`
for the multipart upload.

.. code-block:: php

    $source = '/path/to/large/file.zip';
    $uploader = new MultipartUploader($s3Client, $source, [
        'bucket' => 'your-bucket',
        'key'    => 'my-file.zip',
    ]);

    $promise = $uploader->promise();

Configuration
=============

The ``MultipartUploader`` object constructor accepts the following arguments:

``$client``
    The ``Aws\ClientInterface`` object to use for performing the transfers.
    This should be an instance of ``Aws\S3\S3Client``.

``$source``
    The source data being uploaded. This can be a path or URL (e.g.,
    ``/path/to/file.jpg``), a resource handle (e.g., ``fopen('/path/to/file.jpg', 'r)``),
    or an instance of a :aws-php-class:`PSR-7 stream </class-Psr.Http.Message.StreamInterface.html>`.

``$config``
    An associative array of configuration options for the multipart upload.

    The following configuration options are valid:
    
    **acl**
        (``string``) Access control list (ACL) to set on the object being upload. Objects are private by
        default.
    **before_complete**
        (``callable``) Callback to invoke before the ``CompleteMultipartUpload``
        operation. The callback should have a function signature like
        ``function (Aws\Command $command) {...}``.
    **before_initiate**
        (``callable``) Callback to invoke before the ``CreateMultipartUpload``
        operation. The callback should have a function signature like
        ``function (Aws\Command $command) {...}``.
    **before_upload**
        (``callable``) Callback to invoke before any ``UploadPart`` operations. The
        callback should have a function signature like
        ``function (Aws\Command $command) {...}``.
    **bucket**
        (``string``, *required*) Name of the bucket to which the object is being uploaded.
    **concurrency**
        (``int``, *default*: ``int(5)``) Maximum number of concurrent ``UploadPart``
        operations allowed during the multipart upload.
    **key**
        (``string``, *required*) Key to use for the object being uploaded.
    **part_size**
        (``int``, *default*: ``int(5242880)``) Part size, in bytes, to use when doing a
        multipart upload. This must between 5 MB and 5 GB, inclusive.
    **state**
        (``Aws\Multipart\UploadState``) An object that represents the state of the
        multipart upload and that is used to resume a previous upload. When this
        option is provided, the ``bucket``, ``key``, and ``part_size`` options
        are ignored.
    
Multipart Copies
================

The |sdk-php| also includes a ``MultipartCopy`` object that is used in a similar way
to the ``MultipartUploader``, but is designed for copying objects between 5 GB and
5 TB in size within |S3|.

.. code-block:: php

    use Aws\S3\MultipartCopy;
    use Aws\Exception\MultipartUploadException;

    $copier = new MultipartCopy($s3Client, '/bucket/key?versionId=foo', [
        'bucket' => 'your-bucket',
        'key'    => 'my-file.zip',
    ]);

    try {
        $result = $copier->copy();
        echo "Copy complete: {$result['ObjectURL']}\n";
    } catch (MultipartUploadException $e) {
        echo $e->getMessage() . "\n";
    }
