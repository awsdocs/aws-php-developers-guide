.. Copyright 2010-2018
 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################################################
Getting Your Account\-Specific Endpoint for |EMClong| with |sdk-php| Version 3
##############################################################################

.. meta::
   :description: Example that shows how to get the account-specific endpoint for use with AWS Elemental MediaConvert. using the AWS SDK for PHP version 3.
   :keywords: AWS Elemental MediaConvert Examples

In this example, you use the |sdk-php| Version 3 to call |EMClong| and retrieve your account-specific endpoint. You can retrieve your endpoint 
URL from the service default endpoint and so do not yet need your acccount-specific endpoint. 

The following examples show how to:

* Retrieve your account-specific endpoint. using :aws-php-class:`DescribeEndpoints </api-mediaconvert-2017-08-29.html#describeendpoints>`.

.. include:: text/git-php-examples.txt

To access the |EMC| client, create an |IAM| role that gives |EMClong| access to your input files and the |S3| buckets where your output files are 
stored. For details, see :EMC-ug:`Set Up IAM Permissions <iam-role>` in the |EMC-ug|_.

Retrieve Endpoints
==================

Create an object to pass the empty request parameters for the describeEndpoints method of the ``AWS.MediaConvert`` client class. To call the 
describeEndpoints method, create a promise for invoking an |EMClong| service object, passing the parameters. Handle the response in the promise callback.

**Imports**

.. literalinclude:: example_code/mediaconvert/GetEndpoint.php
   :lines: 19-22
   :language: PHP
   
   

**Sample Code**

Define the region in which to get the endpoint, and create an |EMC| client object:

.. literalinclude:: example_code/mediaconvert/GetEndpoint.php
   :lines: 32-36
   :language: php
   

Call the describeEndpoints method to retrieve the endpoints and save the endpoint's URL:

.. literalinclude:: example_code/mediaconvert/GetEndpoint.php
   :lines: 39-49
   :language: php
   





