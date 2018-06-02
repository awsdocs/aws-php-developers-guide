.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

======================================================
Using Assume Role Credentials
======================================================

.. meta::
   :description: How to load credentials for AWS using the AWS SDK for PHP.
   :keywords: configuration, specify region, region, credentials, proxy
   
If you use ``Aws\Credentials\AssumeRoleCredentialProvider`` to create credentials by assuming a role,
you need to provide ``'client'`` information with an ``StsClient`` object and
``'assume_role_params'`` details.

For more information regarding ``'assume_role_params'``, see :aws-php-class:`AssumeRole </api-sts-2011-06-15.html#assumerole>`.

.. code-block:: php

    $assumeRoleCredentials = new AssumeRoleCredentialProvider([
        'client' => new StsClient([
            'region' => 'us-west-2',
            'version' => '2011-06-15'
        ]),
        'assume_role_params' => [
            'RoleArn' => '<string>', // REQUIRED
            'RoleSessionName' => '<string>', // REQUIRED
            ...
        ]
    ]);