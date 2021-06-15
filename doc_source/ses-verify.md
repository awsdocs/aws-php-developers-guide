# Verifying Email Identities Using the Amazon SES API and the AWS SDK for PHP Version 3<a name="ses-verify"></a>

When you first start using your Amazon Simple Email Service \(Amazon SES\) account, all senders and recipients must be verified in the same AWS Region that you are sending emails to\. For more information about sending emails, see [Sending Email with Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-email.html)\.

The following examples show how to:
+ Verify an email address using [VerifyEmailIdentity](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#verifyemailidentity)\.
+ Verify an email domain using [VerifyDomainIdentity](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#verifydomainidentity)\.
+ List all email addresses using [ListIdentities](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#listidentities)\.
+ List all email domains using [ListIdentities](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#listidentities)\.
+ Remove an email address using [DeleteIdentity](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deleteidentity)\.
+ Remove an email domain using [DeleteIdentity](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deleteidentity)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using Amazon SES, see the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\.

## Verifying Email addresses<a name="verifying-email-addresses"></a>

Amazon SES can send email only from verified email addresses or domains\. By verifying an email address, you demonstrate that you’re the owner of that address and want to allow Amazon SES to send email from that address\.

When you run the following code example, Amazon SES sends an email to the address you specified\. When you \(or the recipient of the email\) click the link in the email, the address is verified\.

To add an email address to your Amazon SES account, use the [VerifyEmailIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailIdentity.html) operation\.

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

$email = 'email_address';

try {
    $result = $SesClient->verifyEmailIdentity([
        'EmailAddress' => $email,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Verify an Email Domain<a name="verify-an-email-domain"></a>

Amazon SES can send email only from verified email addresses or domains\. By verifying a domain, you demonstrate that you’re the owner of that domain\. When you verify a domain, you allow Amazon SES to send email from any address on that domain\.

When you run the following code example, Amazon SES provides you with a verification token\. You have to add the token to your domain’s DNS configuration\. For more information, see [Verifying a Domain with Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domain-procedure.html) in the Amazon Simple Email Service Developer Guide\.

To add a sending domain to your Amazon SES account, use the [VerifyDomainIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyDomainIdentity.html) operation\.

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

$domain = 'domain.name';

try {
    $result = $SesClient->verifyDomainIdentity([
        'Domain' => $domain,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List Email Addresses<a name="list-email-addresses"></a>

To retrieve a list of email addresses submitted in the current AWS Region, regardless of verification status, use the [ListIdentities](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentities.html) operation\.

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
    $result = $SesClient->listIdentities([
        'IdentityType' => 'EmailAddress',
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List Email Domains<a name="list-email-domains"></a>

To retrieve a list of email domains submitted in the current AWS Region, regardless of verification status use the [ListIdentities](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentities.html) operation\.

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
    $result = $SesClient->listIdentities([
        'IdentityType' => 'Domain',
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete an Email Address<a name="delete-an-email-address"></a>

To delete a verified email address from the list of identities, use the [DeleteIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentity.html) operation\.

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

$email = 'email_address';

try {
    $result = $SesClient->deleteIdentity([
        'Identity' => $email,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete an Email Domain<a name="delete-an-email-domain"></a>

To delete a verified email domain from the list of verified identities, use the [DeleteIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentity.html) operation\.

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

$domain = 'domain.name';

try {
    $result = $SesClient->deleteIdentity([
        'Identity' => $domain,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```