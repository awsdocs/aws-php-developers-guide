.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

============================================
Using Credentials from Environment Variables
============================================

.. meta::
   :description: How to load credentials for AWS using the AWS SDK for PHP.
   :keywords: configuration, specify region, region, credentials, proxy


   
For security, and to prevent you from accidentally sharing your secret key we do not recommend 
that you add your AWS access keys directly to the client in any production files.


To authenticate to |AWSlong| the SDK will first check for credentials is in your environment
variables. The SDK uses the ``getenv()`` function function to look for the
``AWS_ACCESS_KEY_ID``, ``AWS_SECRET_ACCESS_KEY``, and ``AWS_SESSION_TOKEN``
environment variables. These credentials are referred to as
**environment credentials**.

If you are hosting your application on  :AEB-dg:`AWS Elastic Beanstalk <create_deploy_PHP_eb>`,
you can set the ``AWS_ACCESS_KEY_ID`` and ``AWS_SECRET_KEY`` environment variables through the 
|AEBlong| console so that the SDK can use those credentials automatically.

You can also set the enviroment variables in the command line as described below:


**Linux**
   
.. code-block:: ini

   $ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
      # The access key for your AWS account.
   $ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
      # The secret key for your AWS account.
   $ export AWS_SESSION_TOKEN=AQoDYXdzEJr...<remainder of security token>
      # The session key for your AWS account. This is only needed when you are using temporary credentials. 
      # The AWS_SECURITY_TOKEN environment variable can also be used, but is only supported for backwards compatibility purposes. 
      # AWS_SESSION_TOKEN is supported by multiple AWS SDKs besides python.


**Windows**
   
.. code-block:: ini

   C:\> SET  AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
      # The access key for your AWS account.
   C:\> SET  AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
      # The secret key for your AWS account.
   C:\> SET AWS_SESSION_TOKEN=AQoDYXdzEJr...<remainder of security token>
      # The session key for your AWS account. This is only needed when you are using temporary credentials. 
      # The AWS_SECURITY_TOKEN environment variable can also be used, but is only supported for backwards compatibility purposes. 
      # AWS_SESSION_TOKEN is supported by multiple AWS SDKs besides python.
  