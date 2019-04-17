
.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

######################################################
Using the AWS Credentials File and Credential Profiles
######################################################

.. meta::
   :description: How to retrieve credentials for AWS using the AWS SDK for PHP.
   :keywords: configuration, specify region, region, credentials, proxy

.. _credential_profiles:

A credentials file is a plaintext file that contains your access keys.
The file must:

* Be on the same machine on which you're running your application.
* Be named :file:`credentials`.
* Be located in the :file:`.aws/` folder in your home directory.

The home directory can vary by
operating system. On Windows, you can refer to your home directory by
using the environment variable :code:`%UserProfile%`. On Unix-like systems, you
can use the environment variable :code:`$HOME` or :code:`~` (tilde).

If you already use this file for other SDKs and tools (like the |CLI|),
you don't need to change anything to use the files in this SDK. If
you use different credentials for different tools or applications, you
can use **profiles** to configure multiple access keys in the same
configuration file.

We use this method in all our PHP code examples.

Using an AWS credentials file offers the following benefits:

* Your projects' credentials are stored outside of your projects, so there is
   no chance of accidentally committing them into version control.
* You can define and name multiple sets of credentials in one place.
* You can easily reuse the same credentials among projects.
* Other AWS SDKs and tools support, this same
   credentials file. This allows you to reuse your credentials with other
   tools.

The format of the AWS credentials file should look something like the
following.

.. code-block:: ini

    [default]
    aws_access_key_id = YOUR_AWS_ACCESS_KEY_ID
    aws_secret_access_key = YOUR_AWS_SECRET_ACCESS_KEY

    [project1]
    aws_access_key_id = ANOTHER_AWS_ACCESS_KEY_ID
    aws_secret_access_key = ANOTHER_AWS_SECRET_ACCESS_KEY

Each section (e.g., ``[default]``, ``[project1]``), represents a separate
credential profile. You can reference profiles from an SDK configuration
file, or when you are instantiating a client, by using the ``profile`` option.

.. code-block:: php

    use Aws\DynamoDb\DynamoDbClient;

    // Instantiate a client with the credentials from the project1 profile
    $client = new DynamoDbClient([
        'profile' => 'project1',
        'region'  => 'us-west-2',
        'version' => 'latest'
    ]);

If no credentials or profiles were explicitly provided to the SDK and no
credentials were defined in environment variables, but a credentials file is
defined, the SDK uses the "default" profile. You can change the default
profile by specifying an alternate profile name in the ``AWS_PROFILE``
environment variable.

Assume Role with Profile
========================

You can configure the AWS SDK for PHP to use an IAM role by defining a profile for the role in the :file:`config` inside the :file:`.aws/` folder.

Create a new profile and add the :code:`role_arn` for the role you will assume and the profile 
from the credentials file that has permissions to assume this role. 


.. code-block:: ini

    [profile test_role]
    role_arn = arn:aws:iam::123456789012:role/testing
    source_profile = project1
