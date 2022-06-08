# Requirements and Recommendations for the AWS SDK for PHP Version 3<a name="getting-started_requirements"></a>

For best results with AWS SDK for PHP, ensure your environment supports the following requirements and recommendations\.

## Requirements<a name="requirements"></a>

To use the AWS SDK for PHP, you must be using PHP version 5\.5\.0 or later with the [SimpleXML PHP extension](https://www.php.net/manual/en/book.simplexml.php) enabled\. If you need to sign private Amazon CloudFront URLs, you also need the [OpenSSL PHP extension](http://php.net/manual/en/book.openssl.php)\.

## Recommendations<a name="recommendations"></a>

In addition to the minimum requirements, we recommend you also install, uninstall, and use the following\.


****  

|  |  | 
| --- |--- |
|  Install [cURL](http://php.net/manual/en/book.curl.php) 7\.16\.2 or later  |  Use a recent version of cURL compiled with OpenSSL/NSS and zlib\. If cURL isn’t installed on your system and you don’t configure a custom http\_handler for your client, the SDK uses the PHP stream wrapper\.  | 
|  Use [OPCache](http://php.net/manual/en/book.opcache.php)   |  Use the OPcache extension to improve PHP performance by storing precompiled script bytecode in shared memory\. This removes the need for PHP to load and parse scripts on each request\. This extension is typically enabled by default\. When running Amazon Linux, you need to install the php56\-opcache or php55\-opcache yum package to use the OPCache extension\.  | 
|  Uninstall [Xdebug](http://xdebug.org/)   |  Xdebug can help identify performance bottlenecks\. However, if performance is critical to your application, don’t install the Xdebug extension in your production environment\. Loading the extension slows SDK performance considerably\.  | 
|  Use a [Composer](http://getcomposer.org) classmap autoloader  |  Autoloaders load classes as they are required by a PHP script\. Composer generates an autoloader that can autoload the PHP scripts of your application and all other PHP scripts required by your application, including the AWS SDK for PHP\. For production environments, we recommended you use a classmap autoloader to improve autoloader performance\. You can generate a classmap autoloader by passing the `-o` or `==optimize-autoloader` option to Composer’s install command\.  | 

## Compatibility Test<a name="compatibility-test"></a>

Run the `compatibility-test.php` file in the SDK to verify your system can run the SDK\. In addition to meeting the SDK’s minimum system requirements, the compatibility test checks for optional settings and makes recommendations that can help improve performance\. The compatibility test outputs results either to the command line or a web browser\. When reviewing test results in a browser, successful checks appear in green, warnings in purple, and failures in red\. When running from the command line, the result of a check appears on a separate line\.

When reporting an issue with the SDK, sharing the output of the compatibility test helps identify the underlying cause\.
