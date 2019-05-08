RDS

shared entities - https://github.com/awsdocs/aws-doc-shared-content/blob/master/sphinx_shared/_includes/common_includes.txt
|RDSlong|
|RDS|
:rds-api:
|rds-dg|

aws-php-class - https://docs.aws.amazon.com/aws-sdk-php/v3/api/ 

literalinclude - https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code 

.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###############################################################################
|RDSlong| the |RDS| API and the |sdk-php| Version 3
###############################################################################

.. meta::
   :description: Amazon RDS code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon RDS code examples for PHP, 

|RDSlong| (|RDS|) allows you to create scalable relational databases. Using |RDS| you can create a database server for
MySQL, MariaDB, PostgreSQL, Microsoft SQL Server, Oracle, or Amazon Aurora that will scale to meet demand. The following
examples show you how to create and maintain a MySQL database with |RDS|.


The following examples show how to:

* Create Database Instance using :aws-php-class:`createDBInstance <api-rds-2014-09-01.html#createdbinstance>`.
* Create Database Read Replica using :aws-php-class:`CreateDBInstanceReadReplica <api-rds-2014-09-01.html#createdbinstancereadreplica>`.
* Create Database Snapshot using :aws-php-class:`CreateDBSnapshot <api-rds-2014-09-01.html#createdbsnapshot>`.
* Get information about an Instance using :aws-php-class:`DescribeDBInstances <api-rds-2014-09-01.html#describedbinstances>`.
* Get logs from an Instance using :aws-php-class:`DescribeEvents <api-rds-2014-09-01.html#describeevents>`.
* Start an Database instance using :aws-php-class:`StartDBInstance <api-rds-2014-10-31.html#startdbinstance>`.
* Stop an Database instance using :aws-php-class:`StopDBInstance <api-rds-2014-10-31.html#stopdbinstance>`.
* Delete Instance using :aws-php-class:`DeleteDBInstance  <api-rds-2014-09-01.html#deletedbinstance>`.

.. include:: text/git-php-examples.txt

Create a |RDS| Database Instance
================================

To create a |RDS| Database instance, use the :aws-php-class:`createDBInstance <api-rds-2014-09-01.html#createdbinstance>` operation.

**Imports**

.. literalinclude::  RDS.php.create_db_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.create_db_instance.main.txt
   :language: PHP

Create a |RDS| Database Instance Read Replica
=============================================

To create a |RDS| Database read replica of an existing |RDS| database, use the :aws-php-class:`CreateDBInstanceReadReplica <api-rds-2014-09-01.html#createdbinstancereadreplica>` operation.

**Imports**

.. literalinclude::  RDS.php.create_db_replica.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.create_db_replica.main.txt
   :language: PHP
   
Create a |RDS| Database SnapShot
================================

To create a |RDS| Database snapshot, use the :aws-php-class:`CreateDBSnapshot  <api-rds-2014-09-01.html#createdbsnapshot>` operation.

**Imports**

.. literalinclude::  RDS.php.create_db_snapshot.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.create_db_snapshot.main.txt
   :language: PHP
   
Retrieve a |RDS| Instance Details
=================================

To retrieve information about provisioned |RDS| instances, use the :aws-php-class:`DescribeDBInstances  <api-rds-2014-09-01.html#describedbinstances>` operation.

**Imports**

.. literalinclude:: RDS.php.describe_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.describe_instance.main.txt
   :language: php
   
Retrieve a |RDS| Instance Logs
=================================

To retrieve the last hour of logs from a provisioned |RDS| database instances, use the :aws-php-class:`DescribeEvents  <api-rds-2014-09-01.html#describeevents>` operation.

**Imports**

.. literalinclude:: RDS.php.describe_events.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.describe_events.main.txt
   :language: php

Start |RDS| Instance
=======================

To start a |RDS| database instance that is currently stopped, use the :aws-php-class:`StartDBInstance <api-rds-2014-10-31.html#startdbinstance>` operation.

**Imports**

.. literalinclude:: RDS.php.start_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.start_instance.main.txt
   :language: php


Stop a |RDS| Instance
==========================

To stop a specified |RDS| database instance, use the :aws-php-class:`StopDBInstance <api-rds-2014-10-31.html#stopdbinstance>` operation.

**Imports**

.. literalinclude:: RDS.php.stop_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.stop_instance.main.txt
   :language: php

Delete a |RDS| Instance 
==========================

To remove a specified |RDS| Database Instance , use the :aws-php-class:`DeleteDBInstance  <api-rds-2014-09-01.html#deletedbinstance>` operation.

**Imports**

.. literalinclude:: RDS.php.delete_instance.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: RDS.php.delete_instance.main.txt
   :language: php
   
Related Information
===================

The |sdk-php| examples use the following REST operations from the |rds-api|: 

* :rds-api:`createDBInstance <API_CreateDBInstance>`
* :rds-api:`CreateDBInstanceReadReplica <API_CreateDBInstanceReadReplica>`
* :rds-api:`CreateDBSnapshot <API_CreateDBSnapshot>`
* :rds-api:`DescribeDBInstances <API_DescribeDBInstances>`
* :rds-api:`DescribeEvents <API_DescribeEvents>`
* :rds-api:`StartDBInstance <API_StartDBInstance>`
* :rds-api:`StopDBInstance <API_StopDBInstance>`
* :rds-api:`DeleteDBInstance <API_DeleteDBInstance>` 
   
For more information about using |RDSlong|, see the |rds-dg|_.