.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

######################################################################
Managing Email Filters Using the |SES| API and the |sdk-php| Version 3
######################################################################

.. meta::
   :description: Use the Amazon SES API to manage email filters.
   :keywords: Amazon SES code examples for PHP, IP Address Email Filters with PHP

In addition to sending emails, you can also receive email with |SESlong| (|SES|). An IP address filter enables you to optionally specify whether to accept or reject mail that originates from an IP address or range of IP addresses. For more information, see :SES-dg:`Managing IP Address Filters for Amazon SES Email Receiving <receiving-email-managing-ip-filters>`.

The following examples show how to:

* Create an email filter using :aws-php-class:`CreateReceiptFilter <api-email-2010-12-01.html#createreceiptfilter>`.
* List all email filters using :aws-php-class:`ListReceiptFilters <api-email-2010-12-01.html#listreceiptfilters>`.
* Remove an email filter using :aws-php-class:`DeleteReceiptFilter <api-email-2010-12-01.html#deletereceiptfilter>`.

.. include:: text/git-php-examples.txt

For more information about using |SES|, see the |SES-dg|_.

Create an Email Filter
======================

To allow or block emails from a specific IP address, use the :SES-api:`CreateReceiptFilter <API_CreateReceiptFilter>` operation. Provide the IP address or range of addresses and a unique name to identify this filter.

**Imports**

.. literalinclude::  example_code/ses/Create_Filter.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Filter.php
   :lines: 26-
   :language: php

List All Email Filters
======================

To list the IP address filters associated with your AWS account in the current AWS Region, use the :SES-api:`ListReceiptFilters <API_ListReceiptFilters>` operation.


**Imports**

.. literalinclude::  example_code/ses/List_Filters.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/List_Filters.php
   :lines: 26-
   :language: php

Delete an Email Filter
======================

To remove an existing filter for a specific IP address use the :SES-api:`DeleteReceiptFilter <API_DeleteReceiptFilter>` operation. Provide the unique filter name to identify the receipt filter to delete.

If you need to change the range of addresses that are filtered, you can delete a receipt filter and create a new one.

**Imports**

.. literalinclude::  example_code/ses/Delete_Filter.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Filter.php
   :lines: 26-
   :language: php
