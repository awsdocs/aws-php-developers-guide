.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################
Credentials for the |sdk-php| Version 3
#######################################

.. meta::
   :description: How to load credentials for AWS using the AWS SDK for PHP.
   :keywords: configuration, specify region, region, credentials, proxy

To make requests to |AWSlong|, you must supply :iam-ug:`AWS access keys <id_credentials_access-keys>`, also known as credentials,
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

Using the Default Credential Provider Chain
===========================================

When you initialize a new service client without providing any
credential arguments, the SDK uses the default credential provider
chain to find AWS credentials. The SDK uses the first provider
in the chain that returns credentials without an error.


The default provider chain
looks for and uses credentials as follows, in this order:


1. :doc:`Use credentials from environment variables <guide_credentials_environment>`.

   Setting environment variables is useful if you're doing development
   work on a machine other than an |EC2| instance.


2. :doc:`Use the AWS shared credentials file and profiles <guide_credentials_profiles>`.

   This credentials file is the same one used by other SDKs and the |CLI|.
   If you're already using a shared credentials file, you can use
   that file for this purpose.

   We use this method in most of our PHP code examples.

3. :doc:`Assume an IAM role <guide_credentials_assume_role>`.

   |IAM| roles provide applications on the instance with temporary security
   credentials to make AWS calls. For example, |IAM| roles offer an easy way to
   distribute and manage credentials on multiple |EC2| instances.


.. _other_credentials:

Other Ways to Add Credentials
=============================

You can also add credentials in these ways:


* :doc:`Using a credential provider <guide_credentials_provider>`.

   Provide custom logic for credentials when constructing the client.

* :doc:`Using temporary credentials from AWS STS <guide_credentials_temporary>`.

   When using a multi-factor authentication (MFA) token for two-factor authentication, use |STS| to
   give the user temporary crentials to access AWS services or use the |sdk-php|.


* :doc:`Using hard-coded credentials <guide_credentials_hardcoded>` (not recommended).


.. warning::

    Hard-coding your credentials can be dangerous, because it's easy to
    accidentally commit your credentials into an SCM repository. This can potentially
    expose your credentials to more people than you intend. It can also make it
    difficult to rotate credentials in the future. Do not submit code with hard-coded
    credentials to your source control.

* :doc:`Creating anonymous clients <guide_credentials_anonymous>`.

   Create a client that isn't associated with any credentials when the service allows anonymous access.


For more information, see |aws-credentials|_ Best Practices in the |AWS-gr|_.

.. toctree::
    :maxdepth: 1

    Using Credentials from Environment Variables <guide_credentials_environment>
    Using the AWS Credentials File and Credential Profiles <guide_credentials_profiles>
    Assume an IAM Role <guide_credentials_assume_role>
    Using a Credential Provider <guide_credentials_provider>
    Using Temporary Credentials from AWS STS <guide_credentials_temporary>
    Hard code credentials <guide_credentials_hardcoded>
    Creating Anonymous Clients <guide_credentials_anonymous>

