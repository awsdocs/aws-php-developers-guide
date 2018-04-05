.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

=========
|sdk-php|
=========

.. meta::
   :description: AWS SDK for PHP enables PHP developers to use Amazon Web Services in their PHP code.
   :keywords: AWS SDK for PHP , AWS for PHP, Amazon PHP, 

The |sdk-php| enables PHP developers to use
`Amazon Web Services <http://aws.amazon.com/>`_ in their PHP code, and build
robust applications and software using services like |S3|, |DDBlong|, |GL|, etc.
You can get started in minutes by installing the
SDK through Composer — by requiring the ``aws/aws-sdk-php`` package — or by
downloading the standalone `aws.zip <http://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip>`_
or `aws.phar <http://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar>`_ file.

.. image:: images/php-sdk-overview.png
   :alt: Diagram that provides an overview of how AWS SDK for PHP works

Not all services are immediately available in the SDK. To find out which services are currently supported by the |sdk-php|, see :aws-php-class:`Service Name and API Version <index.html>`.
For information about the |sdk-php| on GitHub, see :doc:`Additional Resources <resources>`.

External links: :aws-php-class:`API Docs <index.html>`
| `GitHub <https://github.com/aws/aws-sdk-php>`_
| `Twitter <https://twitter.com/awsforphp>`_
| `Gitter <https://gitter.im/aws/aws-sdk-php>`_
| `Blog <https://aws.amazon.com/blogs/developer/category/php/>`_
| `Forum <https://forums.aws.amazon.com/forum.jspa?forumID=80>`_
| `Packagist <https://packagist.org/packages/aws/aws-sdk-php>`_

.. note::

    If you're migrating your project's code from using Version 2 of the SDK to
    Version 3, be sure to read :doc:`getting-started_migration`.

Getting Started
---------------

*  :doc:`getting-started_requirements`
*  :doc:`getting-started_installation`
*  :doc:`getting-started_basic-usage`
*  :doc:`getting-started_migration`

SDK Guides
----------

* :doc:`guide_configuration`
* :doc:`guide_credentials`
* :doc:`guide_commands`
* :doc:`guide_promises`
* :doc:`guide_handlers-and-middleware`
* :doc:`guide_streams`
* :doc:`guide_paginators`
* :doc:`guide_waiters`
* :doc:`guide_jmespath`


Service-Specific Features
-------------------------

* :doc:`service_cloudsearch-custom-requests`
* :doc:`service_cloudfront-signed-url`
* :doc:`cloud9`
* :doc:`service_dynamodb-session-handler`
* :doc:`service_es-data-plane`
* :doc:`s3-multipart-upload`
* :doc:`s3-multiregion-client`
* :doc:`s3-presigned-post`
* :doc:`s3-presigned-url`
* :doc:`s3-stream-wrapper`
* :doc:`s3-transfer`
* :doc:`s3-service-encryption-client`

Examples
--------
* :doc:`cw-examples`
* :doc:`ec2-examples`
* :doc:`iam-examples`
* :doc:`s3-examples`
* :doc:`sqs-examples`

Reference
---------

* :doc:`faq`
* :doc:`glossary`
* `Contributing to the SDK <https://github.com/aws/aws-sdk-php/blob/master/CONTRIBUTING.md>`_
* `Guzzle Documentation <http://guzzlephp.org>`_

.. _supported-services:

API Documentation
-----------------

Find API documentation for the SDK at  http://docs.aws.amazon.com/aws-sdk-php/v3/api/.
