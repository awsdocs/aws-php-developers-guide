.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################
|S3| Pre-Signed POSTs with |sdk-php| Version 3
##############################################

.. meta::
   :description: Create write access to private Amazon S3 data using the AWS SDK for PHP version 3.
   :keywords: |S3|, AWS SDK for PHP version 3 examples, Amazon S3 for PHP code examples

Much like pre-signed URLs, pre-signed POSTs enable you to give write access to a
user without giving them AWS credentials. Pre-signed POST forms can be created
with the help of an instance of :aws-php-class:`Aws\S3\PostObjectV4 <class-Aws.S3.PostObjectV4.html>`.

The following examples show how to:

* Get data for an S3 Object POST upload form using :aws-php-class:`PostObjectV4 <class-Aws.S3.PostObjectV4.html>`.

.. include:: text/git-php-examples.txt

Create PostObjectV4
===================

To create an instance of ``PostObjectV4``, you must provide the following: 

- instance of ``Aws\S3\S3Client``
- bucket
- associative array of form input fields
- array of policy conditions (see :s3-dg:`Policy Construction <HTTPPOSTForms>` in the |S3-dg|)
- expiration time string for the policy (optional, one hour by default).

**Imports**

.. literalinclude:: s3.php.presigned_post.import.txt
   :language: php

**Sample Code**

.. literalinclude:: s3.php.presigned_post.main.txt
   :language: php

