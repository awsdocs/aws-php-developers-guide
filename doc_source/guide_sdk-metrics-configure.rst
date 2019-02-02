.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#########################################
Set Up |SDKM| for the |sdk-php| Version 3
#########################################

.. meta::
   :description: Configure an agent for AWS SDK Metrics for Enterprise Support with the AWS SDK for PHP version 3.
   :keywords: AWS SDK for PHP version 3, AWS SDK Metrics for Enterprise Support with PHP, use php to monitor AWS Services

The following steps demonstrate how to set up |SDKM| for the |sdk-php|. These steps pertain to an |EC2| instance 
running Amazon Linux for a client application that is using the |sdk-php|.
|SDKM| is also available for your production environments if you enable it while configuring the |sdk-php|. 

To use |SDKM|, run the latest version of the |CW| agent. Learn how to 
:CW-dg:`Configure the CloudWatch Agent for SDK Metrics<Configure-CloudWatch-Agent-SDK-Metrics>` in the |CW-dg|.

For more details about IAM Permissions for |SDKM|, check out the :doc:`IAM Permissions for SDK Metrics for AWS SDK for PHP <guide_sdk-metrics-set-permissions>` article. 

To set up |SDKM| with the |sdk-php|, follow these instructions:

1. Create an application with an |sdk-php| client to use an AWS service.
2. Host your project on an |EC2| instance or in your local environment.
3. Install and use the latest version of the |sdk-php|.
4. Install and configure a |CW| agent on an EC2 instance or in your local environment.
5. Authorize |SDKM| to collect and send metrics. 
6. :ref:`csm-enable-agent`.

For more information, see the following:

* :ref:`csm-update-agent`
* :ref:`csm-disable-agent`


.. _csm-enable-agent:

Enable |SDKM| for the |sdk-php|
===============================

By default, |SDKM| is turned off, and the port is set to 31000. The following are the default parameters.

.. code-block:: ini

    //default values
     [
         'enabled' => false,
         'port' => 31000,
     ]

Enabling |SDKM| is independent of configuring your credentials to use an AWS service.

You can enable |SDKM| by setting environment variables or by using the AWS Shared config file.

Option 1: Set Environment Variables
-----------------------------------

The SDK first checks the profile specified in the environment variable under :code:`AWS_PROFILE` to determine if |SDKM| is enabled.

To turn on |SDKM|, add the following to your environmental variables.

.. code-block:: ini

    export AWS_CSM_ENABLED=true

:ref:`Other configuration settings<csm-update-agent>` are available. For more information about using shared files, see
:doc:`Using Credentials from Environment Variables <guide_credentials_environment>`.

.. note::
    Enabling |SDKM| does not configure your credentials to use an AWS service. 
    To do that, see :doc:`Credentials for the AWS SDK for PHP Version 3<guide_credentials>`.

Option 2: AWS Shared Config File
--------------------------------

If no CSM configuration is found in the environment variables, the SDK looks for your default AWS profile field. If :code:`AWS_DEFAULT_PROFILE` is set to something other than default, update that profile. To enable SDK Metrics, add :code:`csm_enabled` to the shared config file located at :file:`~/.aws/config`.

.. code-block:: ini

    [default]
    csm_enabled = true

    [profile aws_csm]
    csm_enabled = true

:ref:`Other configuration settings<csm-update-agent>` are available. For more information about using AWS Shared files, see
:doc:`Using the AWS Credentials File and Credential Profiles <guide_credentials_profiles>`.

.. note:: 
    Enabling |SDKM| does not configure your credentials to use an AWS service. 
    To do that, see :doc:`Credentials for the AWS SDK for PHP Version 3<guide_credentials>`.

.. _csm-update-agent:

Update a |CW| Agent
===================

To make changes to the port ID, you need to set the values and then restart any AWS jobs that are currently active.

Option 1: Set Environment Variables
-----------------------------------

Most AWS services use the default port. But if the service you want |SDKM| to monitor uses a unique port, add `AWS_CSM_PORT=[port_number]`, to the host's environment variables.

.. code-block:: ini

    export AWS_CSM_ENABLED=true
    export AWS_CSM_PORT=1234


Option 2: AWS Shared Config File
--------------------------------

Most services use the default port. But if your service requires a
unique port ID, add `csm_port = [port_number]` to `~/.aws/config`.

.. code-block:: ini

    [default]
    csm_enabled = false
    csm_port = 1234

    [profile aws_csm]
    csm_enabled = false
    csm_port = 1234

Restart |SDKM|
--------------

To restart a job, run the following commands.

.. code-block:: ini

    amazon-cloudwatch-agent-ctl –a stop;
    amazon-cloudwatch-agent-ctl –a start;


.. _csm-disable-agent:

Disable |SDKM|
==============

To turn off |SDKM|, set `csm_enabled` to `false` in your environment variables, or in your AWS Shared config file located at :file:`~/.aws/config`.
Then restart your |CW| agent so that the changes can take effect.

Set csm_enabled to false
------------------------

**Option 1: Environment Variables**

.. code-block:: ini

    export AWS_CSM_ENABLED=false


**Option 2: AWS Shared Config File**

.. note:: Environment variables override the AWS Shared config file. If |SDKM| is enabled in the environment variables, the |SDKM| remains enabled.

.. code-block:: ini

    [default]
    csm_enabled = false

    [profile aws_csm]
    csm_enabled = false
    
Stop |SDKM| and Restart |CW| agent
----------------------------------

To disable |SDKM|, use the following command. 

.. code-block:: ini

    sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a stop &&
    echo "Done"
    
If you are using other |CW| features, restart |CW| with the following command.

.. code-block:: ini

    amazon-cloudwatch-agent-ctl –a start;
    

