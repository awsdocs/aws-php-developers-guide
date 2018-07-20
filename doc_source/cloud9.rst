.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cloud9-php:

============================================
Using |AC9long| with the |sdk-php| Version 3
============================================

.. meta::
    :description:
        Describes how to use AWS Cloud9 with the AWS SDK for PHP version 3.
	:keywords: 	AWS Cloud9, AWS SDK for PHP version 3 examples,

You can use |AC9long| with the |sdk-php| Version 3 to write, run, and debug your PHP code using just a browser. |AC9| includes tools such as a
code editor, debugger, and terminal. Because the |AC9| IDE is cloud based, you can work on your projects from your office, home,
or anywhere using an internet-connected machine. For general information about |AC9|, see the |AC9-ug|_.

Follow these instructions to set up |AC9| with the |sdk-php|:

* :ref:`cloud9-php-account`
* :ref:`cloud9-php-environment`
* :ref:`cloud9-php-sdk`
* :ref:`cloud9-php-examples`
* :ref:`cloud9-php-run`

.. _cloud9-php-account:

Step 1: Set up Your AWS Account to Use |AC9|
============================================

Start to use |AC9| by signing in to the |AC9| console as an |IAMlong| (|IAM|) entity (for example, an |IAM| user) in your AWS account who
has access permissions for |AC9|.

To set up an |IAM| entity in your AWS account to access |AC9|, and to sign in to the |AC9| console, see
:AC9-ug:`Team Setup for AWS Cloud9 <setup>` in the |AC9-ug|_.

.. _cloud9-php-environment:

Step 2: Set up Your |AC9| Development Environment
=================================================

After you sign in to the |AC9| console, use the console to create an |AC9| development environment.
After you create the environment, |AC9| opens the IDE for that environment.

See :AC9-ug:`Creating an Environment in AWS Cloud9 <create-environment>` in the |AC9-ug|_ for details.

.. note::

      As you create your environment in the console for the first time, we recommend that you choose the option
      to :guilabel:`Create a new instance for environment (EC2)`.
      This option tells |AC9| to create an environment, launch an |EC2| instance, and then connect the new
      instance to the new environment. This is the fastest way to begin using |AC9|.

.. _cloud9-php-sdk:

Step 3: Set up the |sdk-php|
============================

After |AC9| opens the IDE for your development environment, use the IDE to set up the |sdk-php| in your environment, as follows.

#. If the terminal isn't already open in the IDE, open it. On the menu bar in the IDE, choose :guilabel:`Window, New Terminal`.
#. Run the following commands to install the |sdk-php|.

   .. code-block:: sh

      curl -sS https://getcomposer.org/installer | php
      php composer.phar require aws/aws-sdk-php

If the IDE can't find PHP, run the following command to install it. (This command assumes you
chose the option to :guilabel:`Create a new instance for environment (EC2)`, earlier in this topic.)

.. code-block:: sh

   sudo yum -y install php56

.. _cloud9-php-examples:

Step 4: Download Example Code
=============================

Use the terminal you opened in the previous step to download example code for the |sdk-php| into the |AC9| development environment.

To do this, run the following command. This command downloads a copy of all of the code examples
used in the official AWS SDK documentation into your environment's root directory.

.. code-block:: sh

   git clone https://github.com/awsdocs/aws-doc-sdk-examples.git

To find code examples for the |sdk-php|, use the :guilabel:`Environment` window to open the
:file:`ENVIRONMENT_NAME\aws-doc-sdk-examples\php\example_code` directory,
where :file:`ENVIRONMENT_NAME` is the name of your development environment.

To learn how to work with these and other code examples, see :doc:`Code Examples <examples_index>`.

.. _cloud9-php-run:

Step 5: Run and Debug Example Code
==================================

To run code in your |AC9| development environment, see
:AC9-ug:`Run Your Code <build-run-debug>` in the |AC9-ug|_.

To debug code, see
:AC9-ug:`Debug Your Code <build-run-debug>` in the |AC9-ug|_.
