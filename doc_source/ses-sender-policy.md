# Authorizing Senders Using the Amazon SES API and the AWS SDK for PHP Version 3<a name="ses-sender-policy"></a>

To enable another AWS account, AWS Identity and Access Management user, or AWS service to send email through Amazon Simple Email Service \(Amazon SES\) on your behalf, you create a sending authorization policy\. This is a JSON document that you attach to an identity that you own\.

The policy expressly lists who you are allowing to send for that identity, and under which conditions\. All senders, other than you and the entities you explicitly grant permissions to in the policy, are not allowed to send emails\. An identity can have no policy, one policy, or multiple policies attached to it\. You can also have one policy with multiple statements to achieve the effect of multiple policies\.

For more information, see [Using Sending Authorization with Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization.html)\.

The following examples show how to:
+ Create an authorized sender using [PutIdentityPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#createidentitypolicy)\.
+ Retrieve polices for an authorized sender using [GetIdentityPolicies](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#getidentitypolicies)\.
+ List authorized senders using [ListIdentityPolicies](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#listidentitypolicies)\.
+ Revoke permission for an authorized sender using [DeleteIdentityPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deleteidentitypolicy)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

For more information about using Amazon SES, see the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\.

## Create an Authorized Sender<a name="create-an-authorized-sender"></a>

To authorize another AWS account to send emails on your behalf, use an identity policy to add or update authorization to send emails from your verified email addresses or domains\. To create an identity policy, use the [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-1'
]);

$identity = "arn:aws:ses:us-east-1:123456789012:identity/example.com";
$other_aws_account = "0123456789";
$policy = <<<EOT
{
  "Id":"ExampleAuthorizationPolicy",
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AuthorizeAccount",
      "Effect":"Allow",
      "Resource":"$identity",
      "Principal":{
        "AWS":[ "$other_aws_account" ]
      },
      "Action":[
        "SES:SendEmail",
        "SES:SendRawEmail"
      ]
    }
  ]
}
EOT;
$name = "policyName";

try {
    $result = $SesClient->putIdentityPolicy([
        'Identity' => $identity,
        'Policy' => $policy,
        'PolicyName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve Polices for an Authorized Sender<a name="retrieve-polices-for-an-authorized-sender"></a>

Return the sending authorization policies that are associated with a specific email identity or domain identity\. To get the sending authorization for a given email address or domain, use the [GetIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicy.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-1'
]);

$identity = "arn:aws:ses:us-east-1:123456789012:identity/example.com";
$policies = ["policyName"];

try {
    $result = $SesClient->getIdentityPolicies([
        'Identity' => $identity,
        'PolicyNames' => $policies,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List Authorized Senders<a name="list-authorized-senders"></a>

To list the sending authorization policies that are associated with a specific email identity or domain identity in the current AWS Region, use the [ListIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentityPolicies.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-1'
]);

$identity = "arn:aws:ses:us-east-1:123456789012:identity/example.com";


try {
    $result = $SesClient->listIdentityPolicies([
        'Identity' => $identity,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Revoke Permission for an Authorized Sender<a name="revoke-permission-for-an-authorized-sender"></a>

Remove sending authorization for another AWS account to send emails with an email identity or domain identity by deleting the associated identity policy with the [DeleteIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentityPolicy.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-1'
]);

$identity = "arn:aws:ses:us-east-1:123456789012:identity/example.com";
$name = "policyName";

try {
    $result = $SesClient->deleteIdentityPolicy([
        'Identity' => $identity,
        'PolicyName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```