# Using IAM account aliases with AWS SDK for PHP Version 3<a name="iam-examples-using-account-aliases"></a>

If you want the URL for your sign\-in page to contain your company name or other friendly identifier instead of your AWS account ID, you can create an alias for your AWS account ID\. If you create an AWS account alias, your sign\-in page URL changes to incorporate the alias\.

The following examples show how to:
+ Create an alias using [CreateAccountAlias](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#createaccountalias)\.
+ List the alias associated with the AWS account using [ListAccountAliases](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#listaccountaliases)\.
+ Delete an alias using [DeleteAccountAlias](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#deleteaccountalias)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Create an alias<a name="create-an-alias"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Iam\IamClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new IamClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2010-05-08'
]);

try {
    $result = $client->createAccountAlias(array(
        // AccountAlias is required
        'AccountAlias' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## List account aliases<a name="list-account-aliases"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Iam\IamClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new IamClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2010-05-08'
]);

try {
    $result = $client->listAccountAliases();
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete an alias<a name="delete-an-alias"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Iam\IamClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new IamClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2010-05-08'
]);

try {
    $result = $client->deleteAccountAlias(array(
        // AccountAlias is required
        'AccountAlias' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```