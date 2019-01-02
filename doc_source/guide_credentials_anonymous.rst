.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################
Creating Anonymous Clients
##########################

.. meta::
   :description: How to configure anonymous access for AWS Services using the AWS SDK for PHP.
   :keywords:

.. _anonymous_access:
   
In some cases, you might want to create a client that is not associated with any
credentials. This enables you to make anonymous requests to a service.

For example, you can configure both |S3| objects and |CSLong| domains to allow
anonymous access.

To create an anonymous client, you set the ``'credentials'`` option to
``false``.

.. code-block:: php

    $s3Client = new S3Client([
        'version'     => 'latest',
        'region'      => 'us-west-2',
        'credentials' => false
    ]);

    // Makes an anonymous request. The object would need to be publicly
    // readable for this to succeed.
    $result = $s3Client->getObject([
        'Bucket' => 'my-bucket',
        'Key'    => 'my-key',
    ]);
