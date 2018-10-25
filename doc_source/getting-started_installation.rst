.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##################################
Installing the |sdk-php| Version 3
##################################

.. meta::
   :description:  Install the AWS SDK for PHP version 3. 
   :keywords: AWS SDK for PHP version 3, php for aws, install AWS SDK for PHP version 3
   
You can install the |sdk-php| Version 3:

* As a dependency in Composer
* As a prepackaged phar in the SDK
* AS a ZIP file in the SDK

Before you install |sdk-php| Version 3 ensure your environment is using PHP version 5.5 or later. Learn more about :doc:`environment requirements and recommendations <getting-started_requirements>`

Install |sdk-php| as a dependency in Composer
=============================================

`Composer <http://getcomposer.org>`_ is the recommended way to install
the |sdk-php|. Composer is a tool for PHP that manages and installs the dependencies of your project.

For more information on how to install Composer, configure autoloading, and follow other best
practices for defining dependencies, see `getcomposer.org <http://getcomposer.org>`_.


Install Composer
----------------

If Composer is not already in your project,  Download and `install Composer <http://getcomposer.org/download>`_. 

   For **Windows**, download and run the `Composer-Setup.exe <https://getcomposer.org/Composer-Setup.exe>`_. 
   
   For **Linux**, type the following at the command line to install Composer.

   ::

       curl -sS https://getcomposer.org/installer | php 


Add |sdk-php| as a dependency to Composer
-----------------------------------------

If `Composer is already installed globally <https://getcomposer.org/doc/00-intro.md#globally>`_ on your system, run the following in the base directory of your project to install |sdk-php| as a dependency:

   ::

       composer require aws/aws-sdk-php
       
     
Otherwise type this Composer command to install the latest version of the |sdk-php| as a dependency.

   ::

       php -d memory_limit=-1 composer.phar require aws/aws-sdk-php

Add autoloader to your php scripts
----------------------------------
       
To utilize the |sdk-php| in your scripts, add a requirement for the composer autoloader.

   .. code-block:: php

       <?php
          require '/path/to/vendor/autoload.php';
       ?>


Installing by Using the Packaged Phar
=====================================

Each release of the |sdk-php| includes a prepackaged phar (PHP archive) that contains all the classes
and dependencies you need to run the SDK. Additionally, the phar automatically registers a class
autoloader for the |sdk-php| and all its dependencies.

You can `download the packaged phar <http://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar>`_
and include it in your scripts.

.. code-block:: php

    <?php
       require '/path/to/aws.phar';
    ?>


.. note::

    Using PHP with the Suhosin patch is not recommended, but is common on Ubuntu and Debian distributions.
    In this case, you might need to enable the use of phars in the suhosin.ini. If you don't do this,
    including a phar file in your code will cause a silent failure. To modify suhosin.ini, add the
    following line.

    ::

        suhosin.executor.include.whitelist = phar

Installing by Using the ZIP file
================================

The |sdk-php| includes a ZIP file containing all the classes and dependencies you need to run the SDK.
Additionally, the ZIP file includes a class autoloader for the |sdk-php| and its dependencies.

To install the SDK, `download the .zip file <http://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip>`_,
and then extract it into your project at a location you choose. Then include the autoloader in your scripts, as follows.

::

     <?php
        require '/path/to/aws-autoloader.php';
     ?>
