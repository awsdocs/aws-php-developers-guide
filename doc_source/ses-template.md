# Creating custom email templates using the Amazon SES API and the AWS SDK for PHP Version 3<a name="ses-template"></a>

Amazon Simple Email Service \(Amazon SES\) enables you to send emails that are personalized for each recipient by using templates\. Templates include a subject line and the text and HTML parts of the email body\. The subject and body sections can also contain unique values that are personalized for each recipient\.

For more information, see [Sending Personalized Email Using the Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-personalized-email-api.html) in the Amazon Simple Email Service Developer Guide\.

The following examples show how to:
+ Create an email template using [CreateTemplate](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#createtemplate)\.
+ List all email templates using [ListTemplates](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#listtemplates)\.
+ Retrieve an email template using [GetTemplate](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#gettemplate)\.
+ Update an email template using [UpdateTemplate](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#updateTemplate)\.
+ Remove an email template using [DeleteTemplate](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#deletetemplate)\.
+ Send a templated email using [SendTemplatedEmail](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#sendtemplatedemail)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

For more information about using Amazon SES, see the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\.

## Create an email template<a name="create-an-email-template"></a>

To create a template to send personalized email messages, use the [CreateTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateTemplate.html) operation\. The template can be used by any account authorized to send messages in the AWS Region to which the template is added\.

**Note**  
Amazon SES doesn’t validate your HTML, so be sure that *HtmlPart* is valid before sending an email\.

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

$name = 'Template_Name';
$html_body = '<h1>AWS Amazon Simple Email Service Test Email</h1>' .
    '<p>This email was sent with <a href="https://aws.amazon.com/ses/">' .
    'Amazon SES</a> using the <a href="https://aws.amazon.com/sdk-for-php/">' .
    'AWS SDK for PHP</a>.</p>';
$subject = 'Amazon SES test (AWS SDK for PHP)';
$plaintext_body = 'This email was send with Amazon SES using the AWS SDK for PHP.';

try {
    $result = $SesClient->createTemplate([
        'Template' => [
            'HtmlPart' => $html_body,
            'SubjectPart' => $subject,
            'TemplateName' => $name,
            'TextPart' => $plaintext_body,
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Get an email template<a name="get-an-email-template"></a>

To view the content for an existing email template including the subject line, HTML body, and plain text, use the [GetTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetTemplate.html) operation\. Only TemplateName is required\.

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

$name = 'Template_Name';

try {
    $result = $SesClient->getTemplate([
        'TemplateName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## List all email templates<a name="list-all-email-templates"></a>

To retrieve a list of all email templates that are associated with your AWS account in the current AWS Region, use the [ListTemplates](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListTemplates.html) operation\.

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
    $result = $SesClient->listTemplates([
        'MaxItems' => 10,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Update an email template<a name="update-an-email-template"></a>

To change the content for a specific email template including the subject line, HTML body, and plain text, use the [UpdateTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdadteTemplate.html) operation\.

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

$name = 'Template_Name';
$html_body = '<h1>AWS Amazon Simple Email Service Test Email</h1>' .
    '<p>This email was sent with <a href="https://aws.amazon.com/ses/">' .
    'Amazon SES</a> using the <a href="https://aws.amazon.com/sdk-for-php/">' .
    'AWS SDK for PHP</a>.</p>';
$subject = 'Amazon SES test (AWS SDK for PHP)';
$plaintext_body = 'This email was send with Amazon SES using the AWS SDK for PHP.';

try {
    $result = $SesClient->updateTemplate([
        'Template' => [
            'HtmlPart' => $html_body,
            'SubjectPart' => $subject,
            'TemplateName' => $name,
            'TextPart' => $plaintext_body,
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete an email template<a name="delete-an-email-template"></a>

To remove a specific email template, use the [DeleteTemplate](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteTemplate.html) operation\. All you need is the TemplateName\.

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

$name = 'Template_Name';

try {
    $result = $SesClient->deleteTemplate([
        'TemplateName' => $name,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Send an email with a template<a name="send-an-email-with-a-template"></a>

To use a template to send an email to recipients, use the [SendTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) operation\.

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

$template_name = 'Template_Name';
$sender_email = 'email_address';
$recipient_emails = ['email_address'];


try {
    $result = $SesClient->sendTemplatedEmail([
        'Destination' => [
            'ToAddresses' => $verified_recipient_emails,
        ],
        'ReplyToAddresses' => [$sender_email],
        'Source' => $sender_email,

        'Template' => $template_name,
        'TemplateData' => '{ }'
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```