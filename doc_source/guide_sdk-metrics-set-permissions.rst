.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################################################
Authorize |SDKM| to Collect and Send Metrics in the |sdk-php| Version 3
#######################################################################

.. meta::
   :description: Set IAM Policy for AWS SDK Metrics for Enterprise Support with the AWS SDK for PHP version 3.
   :keywords: AWS SDK for PHP version 3, AWS SDK Metrics for Enterprise Support with PHP, use php to monitor AWS Services


To collect metrics from AWS SDKs using |SDKMlong| Enterprise customers must create an |IAM| Role that gives |CW| agent
permission to gather data from their |EC2| instance or production environment.  

Use the following PHP code sample or the AWS Console to create an |IAM| Policy and Role for an |CW| agent to access |SDKM| in your environment.

Learn more about using |SDKM| with |sdk-php| in :doc:`guide_sdk-metrics-configure`. 
For more information about |SDKM| see :CW-dg:`IAM Permissions for SDK Metrics <Set-IAM-Permissions-For-SDK-Metrics>` in the |CW-dg|.

Set Up Access Permissions Using the |sdk-php|
=============================================

Create an IAM role for the instance that has permission for |SSMlong| and |SDKM|.

First, create a policy using :aws-php-class:`CreatePolicy <api-iam-2010-05-08.html#createpolicy>`.
Then create a role using :aws-php-class:`CreateRole  <api-iam-2010-05-08.html#createrole>`.
Finally, attach the policy you created to your new role with
:aws-php-class:`AttachRolePolicy <api-iam-2010-05-08.html#attachrolepolicy>`.

.. literalinclude:: example_code/iam/CreateRole.php
   :lines: 19-
   :language: PHP

Set Up Access Permissions by Using the |IAM| Console
====================================================

Alternatively, you can use the |IAM| console to create a role.

.. topic:: To create a role using the IAM console

1. Go to the `IAM console <https://console.aws.amazon.com/iam>`_, and create a role to use |EC2|.
2. In the navigation pane, choose **Roles**.
3. Choose **Create Role**.
4. Choose **AWS Service**, and then choose **EC2**.
5. Choose **Next: Permissions**.
6. Under **Attach permissions policies**, choose **create policy**.
7. For :guilabel:`Service`, choose **Systems Manager**. For :guilabel:`Actions`, expand :code:`Read`, and choose :code:`GetParameters`. For resources, specify your |CW| agent.
8. Add additional permission.
9. Select **Choose a service**, and then **Enter service manually**. For :guilabel:`Service`, enter :code:`sdkmetrics`.  Select all :code:`sdkmetrics` actions and all resources, and then choose **Review Policy**.
10. Name the :guilabel:`Role` :code:`AmazonSDKMetrics`, and add a description.
11. Choose **Create Role**.




