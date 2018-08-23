.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################################################
Manage IP Address Filters Using the |SESlong| API and the |sdk-php| Version 3
##############################################################################

.. meta::
   :description: Use AWS SES API to verify email address and domains.
   :keywords: AWS SES code examples for PHP, IP Address Email Filters with PHP

When you first start using your Amazon SES account, all senders and recipeints must be verified in the same domain that you 
will be sending emails. For more information about sending emails see :SES-dg:`Managing IP Address Filters for Amazon SES Email Receiving <receiving-email-managing-ip-filters>`


The following examples show how to:

* Create an email filter using :aws-php-class:`CreateReceiptFilter <api-email-2010-12-01.html#createreceiptfilter>`.
* List all email filters using :aws-php-class:`ListReceiptFilters <api-email-2010-12-01.html.html#listreceiptfilters>`.
* Remove an email filter using :aws-php-class:`DeleteReceiptFilter <api-email-2010-12-01.html.html#deletereceiptfilter>`.

.. include:: text/git-php-examples.txt

For more information about using |SESlong|, see the |SES-dg|_.

Create an Email Filter
========================

TBA

**Imports**

.. literalinclude::  example_code/ses/Create_Filter.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Filter.php
   :lines: 26-
   :language: php

List all Email Filters
========================

TBA

**Imports**

.. literalinclude::  example_code/ses/List_Filters.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Filters.php
   :lines: 26-
   :language: php

Delete an Email Filter
========================

TBA

**Imports**

.. literalinclude::  example_code/ses/Delete_Filter.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Filter.php
   :lines: 26-
   :language: php
