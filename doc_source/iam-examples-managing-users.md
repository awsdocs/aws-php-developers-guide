# Managing IAM Users with AWS SDK for PHP Version 3<a name="iam-examples-managing-users"></a>

An IAM user is an entity that you create in AWS to represent the person or service that uses it to interact with AWS\. A user in AWS consists of a name and credentials\.

The following examples show how to:
+ Create a new IAM user using [CreateUser](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#createuser)\.
+ List IAM users using [ListUsers](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#listusers)\.
+ Update an IAM user using [UpdateUser](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#updateuser)\.
+ Retrieve information about an IAM user using [GetUser](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#getuser)\.
+ Delete an IAM user using [DeleteUser](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#deleteuser)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Create an IAM User<a name="create-an-iam-user"></a>

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
    $result = $client->createUser(array(
        // UserName is required
        'UserName' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## List IAM Users<a name="list-iam-users"></a>

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
    $result = $client->listUsers();
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Update an IAM User<a name="update-an-iam-user"></a>

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
    $result = $client->updateUser(array(
        // UserName is required
        'UserName' => 'string1',
        'NewUserName' => 'string'
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Get Information about an IAM User<a name="get-information-about-an-iam-user"></a>

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
    $result = $client->getUser(array(
        'UserName' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete an IAM User<a name="delete-an-iam-user"></a>

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
    $result = $client->deleteUser(array(
        // UserName is required
        'UserName' => 'string'
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```