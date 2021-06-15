# Managing Email Filters Using the Amazon SES API and the AWS SDK for PHP Version 3<a name="ses-filters"></a>

In addition to sending emails, you can also receive email with Amazon Simple Email Service \(Amazon SES\)\. An IP address filter enables you to optionally specify whether to accept or reject mail that originates from an IP address or range of IP addresses\. For more information, see [Managing IP Address Filters for Amazon SES Email Receiving](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-managing-ip-filters.html)\.

The following examples show how to:
+ Create an email filter using [CreateReceiptFilter](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#createreceiptfilter)\.
+ List all email filters using [ListReceiptFilters](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#listreceiptfilters)\.
+ Remove an email filter using [DeleteReceiptFilter](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deletereceiptfilter)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using Amazon SES, see the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\.

## Create an Email Filter<a name="create-an-email-filter"></a>

To allow or block emails from a specific IP address, use the [CreateReceiptFilter](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptFilter.html) operation\. Provide the IP address or range of addresses and a unique name to identify this filter\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new Aws\Ses\SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-2'
]);

$filter_name = 'FilterName';
$ip_address_range = '10.0.0.1/24';

try {
    $result = $SesClient->createReceiptFilter([
        'Filter' => [
            'IpFilter' => [
                'Cidr' => $ip_address_range,
                'Policy' => 'Block|Allow',
            ],
            'Name' => $filter_name,
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List All Email Filters<a name="list-all-email-filters"></a>

To list the IP address filters associated with your AWS account in the current AWS Region, use the [ListReceiptFilters](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptFilters.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new Aws\Ses\SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-2'
]);

try {
    $result = $SesClient->listReceiptFilters([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete an Email Filter<a name="delete-an-email-filter"></a>

To remove an existing filter for a specific IP address use the [DeleteReceiptFilter](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptFilter.html) operation\. Provide the unique filter name to identify the receipt filter to delete\.

If you need to change the range of addresses that are filtered, you can delete a receipt filter and create a new one\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new Aws\Ses\SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-2'
]);

$filter_name = 'FilterName';

try {
    $result = $SesClient->deleteReceiptFilter([
        'FilterName' => $filter_name,

    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```