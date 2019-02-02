.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,S
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

############################################
Using Credentials from Environment Variables
############################################

.. meta::
   :description: How to load credentials for AWS using the AWS SDK for PHP.
   :keywords: configuration, specify region, region, credentials, proxy

.. _environment_credentials:

Using environment variables to contain your credentials prevents you from accidentally sharing your AWS secret access key. 
We recommend that you never add your AWS access keys directly to the client in any production files. Many developers have had 
their account compromised by leaked keys. 

To authenticate to |AWSlong|, the SDK first checks for credentials in your environment
variables. The SDK uses the ``getenv()`` function to look for the
``AWS_ACCESS_KEY_ID``, ``AWS_SECRET_ACCESS_KEY``, and ``AWS_SESSION_TOKEN``
environment variables. These credentials are referred to as
**environment credentials**.

If you're hosting your application on  :AEB-dg:`AWS Elastic Beanstalk <create_deploy_PHP_eb>`,
you can set the ``AWS_ACCESS_KEY_ID`` and ``AWS_SECRET_KEY`` environment variables through the
|AEBlong| console so that the SDK can use those credentials automatically.

You can also set the enviroment variables in the command line, as shown here.


**Linux**

.. code-block:: ini

   $ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
      # The access key for your AWS account.
   $ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
      # The secret access key for your AWS account.
   $ export AWS_SESSION_TOKEN=AQoDYXdzEJr...<remainder of security token>
      # The session key for your AWS account. This is needed only when you are using temporary credentials.
      # The AWS_SECURITY_TOKEN environment variable can also be used, but is only supported for backward compatibility purposes.
      # AWS_SESSION_TOKEN is supported by multiple AWS SDKs other than PHP.


**Windows**

.. code-block:: ini

   C:\> SET  AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
      # The access key for your AWS account.
   C:\> SET  AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
      # The secret access key for your AWS account.
   C:\> SET AWS_SESSION_TOKEN=AQoDYXdzEJr...<remainder of security token>
      # The session key for your AWS account. This is needed only when you are using temporary credentials.
      # The AWS_SECURITY_TOKEN environment variable can also be used, but is only supported for backward compatibility purposes.
      # AWS_SESSION_TOKEN is supported by multiple AWS SDKs besides PHP.
