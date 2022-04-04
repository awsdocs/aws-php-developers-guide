# Working with IAM Policies with AWS SDK for PHP Version 3<a name="iam-examples-working-with-policies"></a>

You grant permissions to a user by creating a policy\. A policy is a document that lists the actions that a user can perform and the resources those actions can affect\. By default, any actions or resources that are not explicitly allowed are denied\. Policies can be created and attached to users, groups of users, roles assumed by users, and resources\.

The following examples show how to:
+ Create a managed policy using [CreatePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#createpolicy)\.
+ Attach a policy to a role using [AttachRolePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#attachrolepolicy)\.
+ Attach a policy to a user using [AttachUserPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#attachuserpolicy)\.
+ Attach a policy to a group using [AttachGroupPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#attachgrouppolicy)\.
+ Remove a role policy using [DetachRolePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#detachrolepolicy)\.
+ Remove a user policy using [DetachUserPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#detachuserpolicy)\.
+ Remove a group policy using [DetachGroupPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#detachgrouppolicy)\.
+ Delete a managed policy using [DeletePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#deletepolicy)\.
+ Delete a role policy using [DeleteRolePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#deleterolepolicy)\.
+ Delete a user policy using [DeleteUserPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#deleteuserpolicy)\.
+ Delete a group policy using [DeleteGroupPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#deletegrouppolicy)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Create a Policy<a name="create-a-policy"></a>

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

$myManagedPolicy = '{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "RESOURCE_ARN"
        },
        {
            "Effect": "Allow",
            "Action": [
            "dynamodb:DeleteItem",
            "dynamodb:GetItem",
            "dynamodb:PutItem",
            "dynamodb:Scan",
            "dynamodb:UpdateItem"
        ],
            "Resource": "RESOURCE_ARN"
        }
    ]
}';

try {
    $result = $client->createPolicy(array(
        // PolicyName is required
        'PolicyName' => 'myDynamoDBPolicy',
        // PolicyDocument is required
        'PolicyDocument' => $myManagedPolicy
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Attach a Policy to a Role<a name="attach-a-policy-to-a-role"></a>

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

$roleName = 'ROLE_NAME';

$policyName = 'AmazonDynamoDBFullAccess';

$policyArn = 'arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess';

try {
    $attachedRolePolicies = $client->getIterator('ListAttachedRolePolicies', ([
        'RoleName' => $roleName,
    ]));
    if (count($attachedRolePolicies) > 0) {
        foreach ($attachedRolePolicies as $attachedRolePolicy) {
            if ($attachedRolePolicy['PolicyName'] == $policyName) {
                echo $policyName . " is already attached to this role. \n";
                exit();
            }
        }
    }
    $result = $client->attachRolePolicy(array(
        // RoleName is required
        'RoleName' => $roleName,
        // PolicyArn is required
        'PolicyArn' => $policyArn
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Attach a Policy to a User<a name="attach-a-policy-to-a-user"></a>

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

$userName = 'USER_NAME';

$policyName = 'AmazonDynamoDBFullAccess';

$policyArn = 'arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess';

try {
    $attachedUserPolicies = $client->getIterator('ListAttachedUserPolicies', ([
        'UserName' => $userName,
    ]));
    if (count($attachedUserPolicies) > 0) {
        foreach ($attachedUserPolicies as $attachedUserPolicy) {
            if ($attachedUserPolicy['PolicyName'] == $policyName) {
                echo $policyName . " is already attached to this role. \n";
                exit();
            }
        }
    }
    $result = $client->attachUserPolicy(array(
        // UserName is required
        'UserName' => $userName,
        // PolicyArn is required
        'PolicyArn' => $policyArn,
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Attach a Policy to a Group<a name="attach-a-policy-to-a-group"></a>

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
    $result = $client->attachGroupPolicy(array(
        // GroupName is required
        'GroupName' => 'string',
        // PolicyArn is required
        'PolicyArn' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Detach a User Policy<a name="detach-a-user-policy"></a>

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
    $result = $client->detachUserPolicy(array(
        // UserName is required
        'UserName' => 'string',
        // PolicyArn is required
        'PolicyArn' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Detach a Group Policy<a name="detach-a-group-policy"></a>

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
    $result = $client->detachGroupPolicy(array(
        // GroupName is required
        'GroupName' => 'string',
        // PolicyArn is required
        'PolicyArn' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete a Policy<a name="delete-a-policy"></a>

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
    $result = $client->deletePolicy(array(
        // PolicyArn is required
        'PolicyArn' => 'string'
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete a Role Policy<a name="delete-a-role-policy"></a>

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
    $result = $client->deleteRolePolicy(array(
        // RoleName is required
        'RoleName' => 'string',
        // PolicyName is required
        'PolicyName' => 'string'
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete a User Policy<a name="delete-a-user-policy"></a>

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
    $result = $client->deleteUserPolicy(array(
        // UserName is required
        'UserName' => 'string',
        // PolicyName is required
        'PolicyName' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete a Group Policy<a name="delete-a-group-policy"></a>

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
    $result = $client->deleteGroupPolicy(array(
        // GroupName is required
        'GroupName' => 'string',
        // PolicyName is required
        'PolicyName' => 'string',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```