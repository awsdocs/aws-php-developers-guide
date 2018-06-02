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
that you add your AWS access keys directly to to your client. When you don't provide credentials 
to a client object at the time of its instantiation, the SDK attempts to find credentials elsewhere. 


The first place the SDK checks for credentials is in your environment
variables. The SDK uses the ``getenv()`` function function to look for the
``AWS_ACCESS_KEY_ID``, ``AWS_SECRET_ACCESS_KEY``, and ``AWS_SESSION_TOKEN``
environment variables. These credentials are referred to as
**environment credentials**.

If you are hosting your application on  :AEB-dg:`AWS Elastic Beanstalk <create_deploy_PHP_eb>`,
you can set the AWS_ACCESS_KEY_ID and AWS_SECRET_KEY environment variables through the 
|AEBlong| console so that the SDK can use those credentials automatically.
   

