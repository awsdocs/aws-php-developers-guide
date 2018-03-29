.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


==========================================
Sending Events to |CWElong|
==========================================

.. meta::
   :description: Create rules and add targets to them, and send custom events to |CWE|.
   :keywords: |CWElong|, |sdk-php| examples

|CWE| delivers a near real-time stream of system events that describe changes in Amazon Web Services (AWS) resources to any of various targets. Using simple rules, you can match events and route them to one or more target functions or streams.

The examples below show how to:

* Create a rule using :aws-php-class:`PutRule </api-events-2015-10-07.html#putrule>`.
* Add targets to a rule using :aws-php-class:`PutTargets </api-events-2015-10-07.html#puttargets>`.
* Send custom events to |CWE| using :aws-php-class:`PutEvents </api-events-2015-10-07.html#putevents>`.

All the example code for the |sdk-php| is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
-----------

Before running the example code, configure your AWS credentials, as described in :doc:`guide_credentials`.

Create a Rule
-------------

**Imports**

.. literalinclude::  example_code/cloudwatchEvents/PutRule.php
   :lines: 15-19
   :language: PHP

**Code**

.. literalinclude:: example_code/cloudwatchEvents/PutRule.php
   :lines: 27-44
   :language: php

Add Targets to a Rule
---------------------

**Imports**

.. literalinclude::  example_code/cloudwatchEvents/PutTargets.php
   :lines: 15-19
   :language: PHP

**Code**

.. literalinclude:: example_code/cloudwatchEvents/PutTargets.php
   :lines: 27-47
   :language: php

Send Custom Events
------------------

**Imports**

.. literalinclude::  example_code/cloudwatchEvents/PutEvents.php
   :lines: 15-19
   :language: PHP

**Code**

.. literalinclude:: example_code/cloudwatchEvents/PutEvents.php
   :lines: 27-49
   :language: php
