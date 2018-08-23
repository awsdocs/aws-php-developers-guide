.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#################################################################################
Create and Manage Email Rules Using the |SESlong| API and the |sdk-php| Version 3
#################################################################################

.. meta::
   :description: Use AWS SES API to verify email address and domains.
   :keywords: AWS SES code examples for PHP, IP Address Email Filters with PHP

When you first start using your Amazon SES account, all senders and recipeints must be verified in the same domain that you 
will be sending emails. For more information about sending emails see 
:SES-dg:`Managing Amazon SES Email Receiving  <receiving-email-managing>`


The following examples show how to:

* Create a receipt rule set using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* Create a receipt rule using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* Describe a receipt rule set using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* Describe a receipt rule using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* List all receipt rule sets using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* Update a receipt rule using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* Remove a receipt rule using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.
* Remove a receipt rule set using :aws-php-class:`Name <api-email-2010-12-01.html#REPLACE>`.

.. include:: text/git-php-examples.txt

For more information about using |SESlong|, see the |SES-dg|_.

Create an Receipt Rule Set
==========================

TBA

**Imports**

.. literalinclude::  example_code/ses/Create_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Rule_Set.php
   :lines: 26-
   :language: php

Create an Receipt Rule 
=======================

TBA

**Imports**

.. literalinclude::  example_code/ses/Create_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Create_Rule.php
   :lines: 26-
   :language: php

Describe an Receipt Rule Set
============================

TBA

**Imports**

.. literalinclude::  example_code/ses/Describe_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Describe_Rule_Set.php
   :lines: 26-
   :language: php

Describe an Receipt Rule 
========================

TBA

**Imports**

.. literalinclude::  example_code/ses/Describe_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Describe_Rule.php
   :lines: 26-
   :language: php
   
List all Receipt Rule Sets
==========================

TBA

**Imports**

.. literalinclude::  example_code/ses/List_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule_Set.php
   :lines: 26-
   :language: php

Update an Receipt Rule 
=======================

TBA

**Imports**

.. literalinclude::  example_code/ses/Update_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Update_Rule.php
   :lines: 26-
   :language: php

Delete an Receipt Rule Set
==========================

TBA

**Imports**

.. literalinclude::  example_code/ses/Delete_Rule_Set.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule_Set.php
   :lines: 26-
   :language: php

Delete an Receipt Rule 
=======================

TBA

**Imports**

.. literalinclude::  example_code/ses/Delete_Rule.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/ses/Delete_Rule.php
   :lines: 26-
   :language: php
