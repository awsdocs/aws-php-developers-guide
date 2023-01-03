# Installing the AWS SDK for PHP Version 3<a name="getting-started_installation"></a>

You can install the AWS SDK for PHP Version 3:
+ As a dependency via Composer
+ As a prepackaged phar of the SDK
+ As a ZIP file of the SDK

Before you install AWS SDK for PHP Version 3 ensure your environment is using PHP version 5\.5 or later\. Learn more about [environment requirements and recommendations](getting-started_requirements.md)\.

**Note**  
Installing the SDK via the \.phar and \.zip methods requires the [Multibyte String PHP extension](https://www.php.net/manual/en/book.mbstring.php) to be installed and enabled separately\.

## Install AWS SDK for PHP as a dependency via Composer<a name="install-sdk-php-as-a-dependency-via-composer"></a>

 [Composer](http://getcomposer.org) is the recommended way to install the AWS SDK for PHP\. Composer is a tool for PHP that manages and installs the dependencies of your project\.

For more information on how to install Composer, configure autoloading, and follow other best practices for defining dependencies, see [getcomposer\.org](http://getcomposer.org)\.

### Install Composer<a name="install-composer"></a>

If Composer is not already in your project, download and [install Composer](http://getcomposer.org/download)\.

For **Windows**, download and run the [Composer\-Setup\.exe](https://getcomposer.org/Composer-Setup.exe)\.

For **Linux**, follow the Command\-line installation on [the Download Composer page](http://getcomposer.org/download)\.

### Add AWS SDK for PHP as a dependency via Composer<a name="add-sdk-php-as-a-dependency-via-composer"></a>

If [Composer is already installed globally](https://getcomposer.org/doc/00-intro.md#globally) on your system, run the following in the base directory of your project to install AWS SDK for PHP as a dependency:

```
composer require aws/aws-sdk-php
```

Otherwise type this Composer command to install the latest version of the AWS SDK for PHP as a dependency\.

```
php -d memory_limit=-1 composer.phar require aws/aws-sdk-php
```

### Add autoloader to your php scripts<a name="add-autoloader-to-your-php-scripts"></a>

To utilize the AWS SDK for PHP in your scripts, include the autoloader in your scripts, as follows\.

```
<?php
   require '/path/to/vendor/autoload.php';
?>
```

## Installing by using the packaged phar<a name="installing-by-using-the-packaged-phar"></a>

Each release of the AWS SDK for PHP includes a prepackaged phar \(PHP archive\) that contains all the classes and dependencies you need to run the SDK\. Additionally, the phar automatically registers a class autoloader for the AWS SDK for PHP and all its dependencies\.

You can [download the packaged phar](https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar) and include it in your scripts\.

```
<?php
   require '/path/to/aws.phar';
?>
```

**Note**  
Using PHP with the Suhosin patch is not recommended, but is common on Ubuntu and Debian distributions\. In this case, you might need to enable the use of phars in the suhosin\.ini\. If you don’t do this, including a phar file in your code will cause a silent failure\. To modify suhosin\.ini, add the following line\.  

```
suhosin.executor.include.whitelist = phar
```

## Installing by using the ZIP file<a name="installing-by-using-the-zip-file"></a>

The AWS SDK for PHP includes a ZIP file containing all the classes and dependencies you need to run the SDK\. Additionally, the ZIP file includes a class autoloader for the AWS SDK for PHP and its dependencies\.

To install the SDK, [download the \.zip file](https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip), and then extract it into your project at a location you choose\. Then include the autoloader in your scripts, as follows\.

```
<?php
   require '/path/to/aws-autoloader.php';
?>
```