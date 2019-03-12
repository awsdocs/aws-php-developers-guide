__SERVICE__
|long|
|__|
:___-api:
|___-dg|

.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################################
Managing |long| Distributions Using the |___| API and the |sdk-php| Version 3
###############################################################################

.. meta::
   :description: Amazon __SERVICE__ code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon __SERVICE__ code examples for PHP, 

|long| Description


The following examples show how to:

* Create _____________ using :aws-php-class:`Create_________ <api-__SERVICE__-2018-11-05.html#create______>`.
* Get ________________ using :aws-php-class:`Get____________ <api-__SERVICE__-2018-11-05.html#get_________>`.
* List _______________ using :aws-php-class:`List___________ <api-__SERVICE__-2018-11-05.html#list________>`.
* Update _____________ using :aws-php-class:`Update_________ <api-__SERVICE__-2018-11-05.html#update______>`.
* Disable ____________ using :aws-php-class:`Disable________ <api-__SERVICE__-2018-11-05.html#disable_____>`.
* Delete _____________ using :aws-php-class:`Delete_________ <api-__SERVICE__-2018-11-05.html#delete______>`.

.. include:: text/git-php-examples.txt

For more information about using |long|, see the |___-dg|_.

Create a |___| Distribution
==========================

To create a |___|, use the :___-api:`CreateDistribution <API_CreateDistribution>` operation.

**Imports**

.. literalinclude::  __SERVICE__.php.creates3distribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: __SERVICE__.php.creates3distribution.main.txt
   :language: PHP

Retrieve a |___| Distribution
============================

To retrieve |___| distribution, use the :___-api:`GetDistribution <API_GetDistribution>` operation.

**Imports**

.. literalinclude:: __SERVICE__.php.getdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: __SERVICE__.php.getdistribution.main.txt
   :language: php

List |___| Distributions
=======================

Get a list of existing |___| using the :___-api:`ListDistributions <API_ListDistributions>` operation.

**Imports**

.. literalinclude:: __SERVICE__.php.listdistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: __SERVICE__.php.listdistribution.main.txt
   :language: php


Update a |___| Distribution
==========================

To update a specified |___| distribution, use the :___-api:`UpdateDistribution <API_UpdateDistribution>` operation.

**Imports**

.. literalinclude:: __SERVICE__.php.updatedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: __SERVICE__.php.updatedistribution.main.txt
   :language: php

Disable a |___| Distribution
============================

To disable the specified |___| distribution, use the :___-api:`DisableDistribution <API_DisableDistribution>` operation.

**Imports**

.. literalinclude:: __SERVICE__.php.disabledistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: __SERVICE__.php.disabledistribution.main.txt
   :language: php

Delete a |___| Distribution
==========================

To remove a specified |___| distribution, use the :___-api:`DeleteDistribution <API_DeleteDistribution>` operation.

**Imports**

.. literalinclude:: __SERVICE__.php.deletedistribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: __SERVICE__.php.deletedistribution.main.txt
   :language: php