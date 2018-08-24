.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

############################
Using Hard-Coded Credentials
############################

.. meta::
   :description: How to supply hardcoded AWS credentials for the AWS SDK for PHP.
   :keywords:

When testing new services or debugging issues, developers often want to include AWS credentials when 
constructing the client. See the following for an example of how to authenticate to AWS, but do so 
with caution. :doc:`Credentials for the AWS SDK for PHP Version 3 <guide_credentials>`  lists  many 
recommended ways to add credentials to your project safely.



.. warning::

    Hard-coding your credentials can be dangerous because it's easy to commit your credentials into 
    an SCM repository accidentally. Adding credentials directly in your production code can potentially 
    expose your credentials to more people than you intend. It can also make it difficult to rotate 
    credentials in the future.
    
    
If you decide to hard-code credentials to an SDK client, provide an associative array of "key",
"secret", and optional "token" key-value pairs to the "credentials" option of
a client constructor.

.. code-block:: php

    // Hard-coded credentials
	$s3Client = new S3Client([
        'version'     => 'latest',
        'region'      => 'us-west-2',
        'credentials' => [
            'key'    => 'my-access-key-id',
            'secret' => 'my-secret-access-key',
        ],
    ]);
