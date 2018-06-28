.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################
Credentials for the |sdk-php| version 3
##########################################

.. meta::
   :description: How to load credentials for AWS using the AWS SDK for PHP.
   :keywords: configuration, specify region, region, credentials, proxy

To make requests to |AWSlong|, you must supply :iam-ug:`AWS access keys <id_credentials_access-keys>`, also called credentials,
to the |sdk-php|.

You can do this in the following ways:

* Use the default credential provider chain :emphasis:`(recommended)`.

* Use a specific credential provider or provider chain (or create your own).

* Supply the credentials yourself. These can be root account credentials, |IAM| credentials,
  or temporary credentials retrieved from |STS|.

.. important:: For security, we *strongly recommend* that you *use IAM
   users* instead of the root account for AWS access. For more information, see :iam-ug:`IAM Best Practices
   <best-practices>` in the |iam-ug|.

.. _default_credential_chain:

Default credential provider chain
==================================

When you initialize a new service client without providing any
credential arguments, the SDK uses the default credential provider
chain to find AWS credentials. The SDK uses the first provider
in the chain that returns credentials without an error. The default provider chain
looks for credentials in the following order:

1. :doc:`Using Credentials from Environment Variables <guide_credentials_environment>`

   Setting environment variables is useful if you're doing development
   work on a machine other than an |EC2| instance.


2. :doc:`Using the AWS Shared Credentials File and Profiles <guide_credentials_profiles>` 

   This credentials file is the same one used by other SDKs and the |CLI|.
   If you're already using a shared credentials file, you can also use
   it for this purpose. 
   
   You will see this method used in most of our PHP code examples. 
   
3. If your application is running on an |EC2| instance, :doc:`use an IAM role for EC2 Instances <guide_credentials_instance_profile>`

   |IAM| roles provide applications on the instance temporary security
   credentials to make AWS calls. |IAM| roles provide an easy way to
   distribute and manage credentials on multiple |EC2| instances.


.. _other_credentials:

Other options for adding credentials
====================================

1. :doc:`Using IAM Roles for Amazon Elastic Container Service Tasks <guide_credentials_ecs>` 

   Specity an |IAM| roles to use for |ECS| tasks
   
2. :doc:`Using Assume Role Credentials <guide_credentials_assume_role>` 

    How to provide credentials from an |IAM| role for an AWS Service during client construction.

3. :doc:`Using a Credential Provider <guide_credentials_provider>` 

   Provide custom logic for credentials when constructing the client. 

4. :doc:`Using Temporary Credentials from AWS STS <guide_credentials_temporary>` 

   When using MFA Token for 2 factor authentication, use |STS| to 
   give the user temporary crentials to access AWS Services or use the |sdk-php|.

5. :doc:`Using Hard-Coded Credentials <guide_credentials_hardcoded>` (not recommended).

   Hard-coding credentials in your application can make it difficult to
   manage and rotate those credentials. Use this method only for small
   personal scripts or testing purposes. Do not submit code with
   credentials to source control.
   
6. :doc:`Creating Anonymous Clients <guide_credentials_anonymous>`


   Create a client not associated with any credntials when the service allows anonymous access.


For more information about Security Credentials  see `AWS Security Crentials Best Practices <https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html>`_ in the Amazon Web Services General Reference.

.. toctree::
    :maxdepth: 1
    
    Using Credentials from Environment Variables <guide_credentials_environment>
    Using the AWS Credentials File and Credential Profiles <guide_credentials_profiles>
    Using IAM Roles for Amazon EC2 Instances <guide_credentials_instance_profile>
    Using IAM Roles for Amazon Elastic Container Service Tasks <guide_credentials_ecs>
    Using Assume Role Credentials <guide_credentials_assume_role>
    Using a Credential Provider <guide_credentials_provider>
    Using Temporary Credentials from AWS STS <guide_credentials_temporary>
    Using Hard-Coded Credentials <guide_credentials_hardcoded>
    Creating Anonymous Clients <guide_credentials_anonymous>