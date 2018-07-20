.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

################################################################
Signing Custom |CSlong| Domain Requests with |sdk-php| Version 3
################################################################

.. meta::
   :description: Making a Cloud Search domain request using the AWS SDK for PHP.
   :keywords: Amazon Cloud Search, AWS SDK for PHP examples, Cloud Search for PHP code examples


|CS| domain requests can be customized beyond what is supported by the |sdk-php|.
In cases where you need to make custom requests to domains
protected by |IAM| authentication, you can use the SDK's credential providers and
signers to sign any :aws-php-class:`PSR-7 request <class-Psr.Http.Message.RequestInterface.html>`.

For example, if you're following :CS-dg:`Cloud Search's Getting Started guide <getting-started>`
and want to use an IAM-protected domain for :CS-dg:`Step 3 <getting-started-search>`,
you would need to sign and execute your request as follows.

.. code-block:: php

    use Aws\Credentials\CredentialProvider;
    use Aws\Signature\SignatureV4;
    use GuzzleHttp\Client;
    use GuzzleHttp\Psr7\Request;

    // Prepare a CloudSearch domain request
    $request = new Request(
        'GET',
        'https://<your-domain>.<region-of-domain>.cloudsearch.amazonaws.com/2013-01-01/search?q=star+wars&return=title'
    );

    // Get your credentials from the environment
    $credentials = call_user_func(CredentialProvider::defaultProvider())->wait();

    // Construct a request signer
    $signer = new SignatureV4('cloudsearch', '<region-of-domain>');

    // Sign the request
    $request = $signer->signRequest($request, $credentials);

    // Send the request
    $response = (new Client)->send($request);
    $results = json_decode($response->getBody());
    if ($results->hits->found > 0) {
        echo $results->hits->hit[0]->fields->title . "\n";
    }
