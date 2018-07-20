.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################
Working with |IAM| Server Certificates with |sdk-php| Version 3
###############################################################

.. meta::
   :description: List, update, and get information about certificates using AWS Identity and Access Management (IAM).
   :keywords: AWS Identity and Access Management (IAM) code examples, AWS SDK for PHP version 3

To enable HTTPS connections to your website or application on AWS, you need an SSL/TLS server certificate. To use a certificate that you obtained from an external provider with your website or application on AWS, you must upload the certificate to |IAM| or import it into |ACMlong|.

The following examples show how to:

* List the certificates stored in |IAM| using :aws-php-class:`ListServerCertificates <api-iam-2010-05-08.html#listservercertificates>`.
* Retrieve information about a certificate using :aws-php-class:`GetServerCertificate <api-iam-2010-05-08.html#getservercertificate>`.
* Update a certificate using :aws-php-class:`UpdateServerCertificate <api-iam-2010-05-08.html#updateservercertificate>`.
* Delete a certificate using :aws-php-class:`DeleteServerCertificate <api-iam-2010-05-08.html#deleteservercertificate>`.

.. include:: text/git-php-examples.txt


List Server Certificates
========================

**Imports**

.. literalinclude::  example_code/iam/ListServerCertificates.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/ListServerCertificates.php
   :lines: 31-43
   :language: php


Retrieve a Server Certificate
=============================

**Imports**

.. literalinclude::  example_code/iam/GetServerCertificate.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/GetServerCertificate.php
   :lines: 31-46
   :language: php


Update a Server Certificate
===========================

**Imports**

.. literalinclude::  example_code/iam/UpdateServerCertificate.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/UpdateServerCertificate.php
   :lines: 31-47
   :language: php


Delete a Server Certificate
===========================

**Imports**

.. literalinclude::  example_code/iam/DeleteServerCertificate.php
   :lines: 19-22
   :language: php

**Sample Code**

.. literalinclude:: example_code/iam/DeleteServerCertificate.php
   :lines: 31-46
   :language: php