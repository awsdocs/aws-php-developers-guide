.. Copyright 2010-2018
 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

############################################################################
Creating and Managing Transcoding Jobs in |EMClong| with |sdk-php| Version 3
############################################################################

.. meta::
   :description: Example that shows how to create and manage conversion jobs in AWS Elemental MediaConvert using the AWS SDK for PHP version 3.
   :keywords: AWS Elemental MediaConvert Examples for PHP, Create MediaConvert Job for PHP

In this example, you use the |sdk-php| Version 3 to call |EMClong| and create a transcoding job. Before you begin, you will need to upload the 
input video to the |S3| bucket you provisioned for input storage. For a list of supported input video codecs and containers, see 
:EMC-ug:`Supported Input Codecs and Containers <reference-codecs-containers-input>` in the |EMC-ug|_.

The following examples show how to:

* Create transcoding jobs in |EMClong|. :aws-php-class:`CreateJob </api-mediaconvert-2017-08-29.html#createjob>`.
* Cancel a transcoding job from |EMClong| queue. :aws-php-class:`CancelJob </api-mediaconvert-2017-08-29.html#canceljob>`
* Retrieve the JSON for a completed transcoding job. :aws-php-class:`GetJob </api-mediaconvert-2017-08-29.html#getjob>`
* Retrieve a JSON array for up to 20 of the most recently created jobs. :aws-php-class:`ListJobs </api-mediaconvert-2017-08-29.html#listjobs>`


.. include:: text/git-php-examples.txt

To access the |EMC| client, create an |IAM| role that gives |EMClong| access to your input files and the |S3| buckets where your output 
files are stored. For details, see :EMC-ug:`Set Up IAM Permissions <iam-role>` in the |EMC-ug|_.

Create a Client
===============
Configure the |sdk-php| by creating a |EMC| client, with the region for your code. In this example, the region is set to us-west-2. Because |EMClong| 
uses custom endpoints for each account, you must also configure the ``AWS.MediaConvert client class`` to use your account-specific endpoint. To do this, 
set the endpoint parameter to your :doc:`account-specific endpoint <emc-examples-getendpoint>`.

**Imports**

.. literalinclude::  example_code/mediaconvert/CreateJob.php
   :lines: 19-22
   :language: PHP

**Sample Code**

.. literalinclude::  example_code/mediaconvert/CreateJob.php
   :lines: 32-37
   :language: php


Defining a Simple Transcoding Job
=================================
Create the JSON that defines the transcode job parameters.

These parameters are detailed. You can use the :console:`AWS Elemental MediaConvert console <mediaconvert>` to generate the 
JSON job parameters by choosing your job settings in the console, and then choosing **Show job JSON** at the bottom of the **Job** section. 
This example shows the JSON for a simple job.

**Sample Code**

.. literalinclude:: example_code/mediaconvert/CreateJob.php
   :lines: 39-162
   :language: php

Create a Job
============

After creating the job parameters JSON, call the createJob method by invoking an ``AWS.MediaConvert service object``, and passing the parameters. 
The ID of the job created is returned in the response data.


**Sample Code**

.. literalinclude:: example_code/mediaconvert/CreateJob.php
   :lines: 164-
   :language: php
   
   
Retrieve a Job
==============

With the JobID returned when you called createjob, you can get detailed descriptions of recent jobs in JSON format. 

**Sample Code**

.. literalinclude:: example_code/mediaconvert/GetJob.php
   :lines: 39-47
   :language: php
   
Cancel a Job
============

With the JobID returned when you called createjob, you can cancel a job while it is still in the queue. You can’t cancel jobs that have 
already started transcoding.

**Sample Code**

.. literalinclude:: example_code/mediaconvert/CancelJob.php
   :lines: 39-47
   :language: php
   
Listing Recent Transcoding Jobs
===============================

Create the parameters JSON, including values to specify whether to sort the list in ASCENDING, or DESCENDING order, the ARN of the job queue 
to check, and the status of jobs to include. This will return up to 20 Jobs. To retrieve the twenty next most recent jobs use the nextToken 
string returned with result.

**Sample Code**

.. literalinclude:: example_code/mediaconvert/ListJobs.php
   :lines: 39-
   :language: php
   
     
