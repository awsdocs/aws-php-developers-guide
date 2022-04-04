# Creating and Managing Email Rules Using the Amazon SES API and the AWS SDK for PHP Version 3<a name="ses-rules"></a>

In addition to sending emails, you can also receive email with Amazon Simple Email Service \(Amazon SES\)\. Receipt rules enable you to specify what Amazon SES does with email it receives for the email addresses or domains you own\. A rule can send email to other AWS services including but not limited to Amazon S3, Amazon SNS, or AWS Lambda\.

For more information, see [Managing Receipt Rule Sets for Amazon SES Email Receiving](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-managing-receipt-rule-sets.html) and [Managing Receipt Rules for Amazon SES Email Receiving](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-managing-receipt-rules.html)\.

The following examples show how to:
+ Create a receipt rule set using [CreateReceiptRuleSet](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#createreceiptruleset)\.
+ Create a receipt rule using [CreateReceiptRule](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#createreceiptrule)\.
+ Describe a receipt rule set using [DescribeReceiptRuleSet](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#describereceiptruleset)\.
+ Describe a receipt rule using [DescribeReceiptRule](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#describereceiptrule)\.
+ List all receipt rule sets using [ListReceiptRuleSets](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#listreceiptrulesets)\.
+ Update a receipt rule using [UpdateReceiptRule](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#updatereceiptrule)\.
+ Remove a receipt rule using [DeleteReceiptRule](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deletereceiptrule)\.
+ Remove a receipt rule set using [DeleteReceiptRuleSet](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deletereceiptruleset)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

For more information about using Amazon SES, see the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\.

## Create a Receipt Rule Set<a name="create-a-receipt-rule-set"></a>

A receipt rule set contains a collection of receipt rules\. You must have at least one receipt rule set associated with your account before you can create a receipt rule\. To create a receipt rule set, provide a unique RuleSetName and use the [CreateReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRuleSet.html) operation\.

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

$name = 'Rule_Set_Name';

try {
    $result = $SesClient->createReceiptRuleSet([
        'RuleSetName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Create a Receipt Rule<a name="create-a-receipt-rule"></a>

Control your incoming email by adding a receipt rule to an existing receipt rule set\. This example shows you how to create a receipt rule that sends incoming messages to an Amazon S3 bucket, but you can also send messages to Amazon SNS and AWS Lambda\. To create a receipt rule, provide a rule and the RuleSetName to the [CreateReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRule.html) operation\.

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

$rule_name = 'Rule_Name';
$rule_set_name = 'Rule_Set_Name';
$s3_bucket = 'Bucket_Name';

try {
    $result = $SesClient->createReceiptRule([
        'Rule' => [
            'Actions' => [
                [
                    'S3Action' => [
                        'BucketName' => $s3_bucket,
                    ],
                ],
            ],
            'Name' => $rule_name,
            'ScanEnabled' => true,
            'TlsPolicy' => 'Optional',
            'Recipients' => ['<string>', ...]
        ],
        'RuleSetName' =>  $rule_set_name,
        
     ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Describe a Receipt Rule Set<a name="describe-a-receipt-rule-set"></a>

Once per second, return the details of the specified receipt rule set\. To use the [DescribeReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeReceiptRuleSet.html) operation, provide the RuleSetName\.

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

$name = 'Rule_Set_Name';

try {
    $result = $SesClient->describeReceiptRuleSet([
        'RuleSetName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Describe a Receipt Rule<a name="describe-a-receipt-rule"></a>

Return the details of a specified receipt rule\. To use the [DescribeReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeReceiptRule.html) operation, provide the RuleName and RuleSetName\.

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

$rule_name = 'Rule_Name';
$rule_set_name = 'Rule_Set_Name';

try {
    $result = $SesClient->describeReceiptRule([
        'RuleName' => $rule_name,
        'RuleSetName' => $rule_set_name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List All Receipt Rule Sets<a name="list-all-receipt-rule-sets"></a>

To list the receipt rule sets that exist under your AWS account in the current AWS Region, use the [ListReceiptRuleSets](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptRuleSets.html) operation\.

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
    $result = $SesClient->listReceiptRuleSets([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Update a Receipt Rule<a name="update-a-receipt-rule"></a>

This example shows you how to update a receipt rule that sends incoming messages to an AWS Lambda function, but you can also send messages to Amazon SNS and Amazon S3\. To use the [UpdateReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateReceiptRule.html) operation, provide the new receipt rule and the RuleSetName\.

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

$rule_name = 'Rule_Name';
$rule_set_name = 'Rule_Set_Name';
$lambda_arn = 'Amazon Resource Name (ARN) of the AWS Lambda function';
$sns_topic_arn = 'Amazon Resource Name (ARN) of the Amazon SNS topic';


try {
    $result = $SesClient->updateReceiptRule([
        'Rule' => [
            'Actions' => [
                'LambdaAction' => [
                    'FunctionArn' => $lambda_arn,
                    'TopicArn' => $sns_topic_arn,
                ],
            ],
            'Enabled' => true,
            'Name' => $rule_name,
            'ScanEnabled' => false,
            'TlsPolicy' => 'Require',
        ],
        'RuleSetName' => $rule_set_name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete a Receipt Rule Set<a name="delete-a-receipt-rule-set"></a>

Remove a specified receipt rule set that isn’t currently disabled\. This also deletes all of the receipt rules it contains\. To delete a receipt rule set, provide the RuleSetName to the [DeleteReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRuleSet.html) operation\.

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

$name = 'Rule_Set_Name';

try {
    $result = $SesClient->deleteReceiptRuleSet([
        'RuleSetName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete a Receipt Rule<a name="delete-a-receipt-rule"></a>

To delete a specified receipt rule, provide the RuleName and RuleSetName to the [DeleteReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRule.html) operation\.

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

$rule_name = 'Rule_Name';
$rule_set_name = 'Rule_Set_Name';

try {
    $result = $SesClient->deleteReceiptRule([
        'RuleName' => $rule_name,
        'RuleSetName' => $rule_set_name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```