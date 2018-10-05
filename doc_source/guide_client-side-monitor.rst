.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################
Client Side Monitoring in the |sdk-php| Version 3
#################################################

.. meta::
   :description: Configure an agent for client side monitoring with the AWS SDK for PHP version 3. 
   :keywords: AWS SDK for PHP version 3, client side monitoring with PHP, use php to monitor AWS Services


Client-Side Monitoring (CSM) allows you to 

Step 1 Install and Configure an Agent
=====================================


Step 2 Enable CSM for |sdk-php|
===============================

By default the Client-Side Monitoring is off. 

It has to be enabled in one of the following ways:

.. code-block:: 
    - Set environment variables:  
    AWS_CSM_ENABLED=true  (required)  
    AWS_CSM_PORT=[port_number]  (optional, defaults to 31000)  
    AWS_CSM_CLIENT_ID=[some_string]  (optional, defaults to empty string)
      
    - Set the config file at "~/.aws/config". Note that the SDK will first check for the profile specified in the environment variable "AWS_PROFILE". If that environment variable isn't set, it will check for the profile "aws_csm".  

    [custom_profile_from_aws_profile]  
    csm_enabled = true  (required)  
    csm_port = [port_number]  (optional, defaults to 31000)  
    csm_clientid = [some_string]  (optional, defaults to empty string)  

    [aws_csm]  
    csm_enabled = true  (required)  
    csm_port = [port_number]  (optional, defaults to 31000)  
    csm_clientid = [some_string]  (optional, defaults to empty string)  

    - The application default values default to off and are: 
     [
         'enabled' => false,
         'port' => 31000,
         'client_id' => ''
     ]




Step 3 Receive Monitoring Events
================================

Once enable the agent will automatically receive messages and pass them along to |CW|. 

If you'd like to monitor these events manually use the following instructions:

View Metrics in |CW|
====================

