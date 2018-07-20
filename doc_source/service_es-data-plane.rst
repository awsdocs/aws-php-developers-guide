.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###########################################################
Signing an |ESlong| Search Request with |sdk-php| Version 3
###########################################################

.. meta::
   :description: Sign and use Amazon Elasticsearch Service with the AWS SDK for PHP.
   :keywords: AWS SDK for PHP Elasticsearch, Amazon Elasticsearch Service for PHP

|ESlong| (|ES|) is a managed service that makes it easy
to deploy, operate, and scale |ESlong|, a popular open-source search and
analytics engine. |ES| offers direct access to the |ESlong| API. This
means that developers can use the tools with which they’re familiar, as well
as robust security options, such as using |IAM| users and roles for access
control. Many |ESlong| clients support request signing, but if you're using
a client that doesn't, you can sign arbitrary PSR-7 requests with the
built-in credential providers and signers of the |sdk-php|.

Signing an |ES| Request
=======================

|ES| uses :AWS-gr:`Signature Version 4 <signature-version-4>`.
This means that you need to sign requests against the service's signing
name (``es``, in this case) and the AWS Region of your |ES| domain. A full list
of Regions supported by |ES| can be found :AWS-gr:`on the AWS Regions and Endpoints
page <rande>` in the |AWS-gr|.
However, in this example, we'll sign requests against an |ES| domain in the
``us-west-2`` region.

You'll need to provide credentials, which you can do either with the SDK's
default provider chain or with any form of credentials described in
:doc:`guide_credentials`. You'll also need a :aws-php-class:`PSR-7 request
</class-Psr.Http.Message.RequestInterface.html>` (assumed in the code below to be named ``$psr7Request``).

.. code-block:: php

    // Pull credentials from the default provider chain
    $provider = Aws\Credentials\CredentialProvider::defaultProvider();
    $credentials = call_user_func($provider)->wait();

    // Create a signer with the service's signing name and Region
    $signer = new Aws\Signature\SignatureV4('es', 'us-west-2');

    // Sign your request
    $signedRequest = $signer->signRequest($psr7Request, $credentials);
