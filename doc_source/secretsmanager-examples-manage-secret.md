# Managing secrets using the Secrets Manager API and the AWS SDK for PHP Version 3<a name="secretsmanager-examples-manage-secret"></a>

AWS Secrets Manager stores and manages shared secrets such as passwords, API keys, and database credentials\. With the Secrets Manager service, developers can replace hard\-coded credentials in deployed code with an embedded call to Secrets Manager\.

Secrets Manager natively supports automatic scheduled credential rotation for Amazon Relational Database Service \(Amazon RDS\) databases, increasing application security\. Secrets Manager can also seamlessly rotate secrets for other databases and third\-party services using AWS Lambda to implement service\-specific details\.

The following examples show how to:
+ Create a secret using [CreateSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#createsecret)\.
+ Retrieve a secret using [GetSecretValue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#getsecretvalue)\.
+ List all of the secrets stored by Secrets Manager using [ListSecrets](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#listsecrets)\.
+ Get details about a specified secret using [DescribeSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#describesecret)\.
+ Update a specified secret using [PutSecretValue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#putsecretvalue)\.
+ Set up a secret rotation using [RotateSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#rotatesecret)\.
+ Mark a secret for deletion using [DeleteSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#deletesecret)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Create a secret in Secrets Manager<a name="create-a-secret-in-asm"></a>

To create a secret in Secrets Manager, use the [CreateSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#createsecret) operation\.

In this example, a user name and password are stored as a JSON string\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

$secretName = '<<{{MySecretName}}>>';
$secret = '{"username":"<<USERNAME>>","password":"<<PASSWORD>>"}';
$description = '<<Description>>';

try {
    $result = $client->createSecret([
        'Description' => $description,
        'Name' => $secretName,
        'SecretString' => $secret,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve a secret from Secrets Manager<a name="retrieve-a-secret-from-asm"></a>

To retrieve the value of a secret stored in Secrets Manager, use the [GetSecretValue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#getsecretvalue) operation\.

In this example, secret is a string that contains the stored value\. If called on the secret we created earlier, this sample outputs `[{\"username\":\"<<USERNAME>>\",\"password\":\"<<PASSWORD>>\"}]`\. Use `json.loads` to access indexed values\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => '<<{{MyRegionName}}>>',
]);

$secretName = '<<{{MySecretName}}>>';

try {
    $result = $client->getSecretValue([
        'SecretId' => $secretName,
    ]);

} catch (AwsException $e) {
    $error = $e->getAwsErrorCode();
    if ($error == 'DecryptionFailureException') {
        // Secrets Manager can't decrypt the protected secret text using the provided AWS KMS key.
        // Handle the exception here, and/or rethrow as needed.
        throw $e;
    }
    if ($error == 'InternalServiceErrorException') {
        // An error occurred on the server side.
        // Handle the exception here, and/or rethrow as needed.
        throw $e;
    }
    if ($error == 'InvalidParameterException') {
        // You provided an invalid value for a parameter.
        // Handle the exception here, and/or rethrow as needed.
        throw $e;
    }
    if ($error == 'InvalidRequestException') {
        // You provided a parameter value that is not valid for the current state of the resource.
        // Handle the exception here, and/or rethrow as needed.
        throw $e;
    }
    if ($error == 'ResourceNotFoundException') {
        // We can't find the resource that you asked for.
        // Handle the exception here, and/or rethrow as needed.
        throw $e;
    }
}
// Decrypts secret using the associated KMS CMK.
// Depending on whether the secret is a string or binary, one of these fields will be populated.
if (isset($result['SecretString'])) {
    $secret = $result['SecretString'];
} else {
    $secret = base64_decode($result['SecretBinary']);
}

// Your code goes here;
```

## List secrets stored in Secrets Manager<a name="list-secrets-stored-in-asm"></a>

Get a list of all the secrets that are stored by Secrets Manager using the [ListSecrets](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#listsecrets) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

try {
    $result = $client->listSecrets([
    ]);
    var_dump($result);
}  catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve details about a secret<a name="retrieve-details-about-a-secret"></a>

Stored secrets contain metadata about rotation rules, when it was last accessed or changed, user\-created tags, and the Amazon Resource Name \(ARN\)\. To get the details of a specified secret stored in Secrets Manager, use the [DescribeSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#describesecret) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

$secretName = '<<{{MySecretName}}>>';

try {
    $result = $client->describeSecret([
        'SecretId' => $secretName,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Update the secret value<a name="update-the-secret-value"></a>

To store a new encrypted secret value in Secrets Manager, use the [PutSecretValue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#putsecretvalue) operation\.

This creates a new version of the secret\. If a version of the secret already exists, add the `VersionStages` parameter with the value in `AWSCURRENT` to ensure that the new value is used when retrieving the value\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

$secretName = '<<{{MySecretName}}>>';
$secret = '{"username":"<<USERNAME>>","password":"<<PASSWORD>>"}';

try {
    $result = $client->putSecretValue([
        'SecretId' => $secretName,
        'SecretString' => $secret,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Rotate the value to an existing secret in Secrets Manager<a name="rotate-the-value-to-an-existing-secret-in-asm"></a>

To rotate the value of an existing secret stored in Secrets Manager, use a Lambda rotation function and the [RotateSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#rotatesecret) operation\.

Before you begin, create a Lambda function to rotate your secret\. The [AWS Code Sample Catalog](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-lambda_functions-secretsmanager.html) currently contains several Lambda code examples for rotating Amazon RDS database credentials\.

**Note**  
For more information about rotating secrets, see [Rotating Your AWS Secrets Manager Secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html) in the AWS Secrets Manager User Guide\.

After you set up your Lambda function, configure a new secret rotation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

$secretName = '<<{{MySecretName}}>>';
$lambda_ARN = 'arn:aws:lambda:us-west-2:123456789012:function:MyTestDatabaseRotationLambda';
$rules = ['AutomaticallyAfterDays' => 30];

try {
    $result = $client->rotateSecret([
        'RotationLambdaARN' => $lambda_ARN,
        'RotationRules' => $rules,
        'SecretId' => $secretName,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

When a rotation is configured, you can implement a rotation using the [RotateSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#rotatesecret) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

$secretName = '<<{{MySecretName}}>>';

try {
    $result = $client->rotateSecret([
        'SecretId' => $secretName,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete a secret from Secrets Manager<a name="delete-a-secret-from-asm"></a>

To remove a specified secret from Secrets Manager, use the [DeleteSecret](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-secretsmanager-2017-10-17.html#deletesecret) operation\. To prevent deleting a secret accidentally, a DeletionDate stamp is automatically added to the secret that specifies a window of recovery time in which you can reverse the deletion\. If the time isn’t specified for the recovery window, the default amount of time is 30 days\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\SecretsManager\SecretsManagerClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SecretsManagerClient([
    'profile' => 'default',
    'version' => '2017-10-17',
    'region' => 'us-west-2'
]);

$secretName = '<<{{MySecretName}}>>';

try {
    $result = $client->deleteSecret([
        'SecretId' => $secretName,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Related information<a name="related-information"></a>

The AWS SDK for PHP examples use the following REST operations from the AWS Secrets Manager API Reference:
+  [CreateSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html) 
+  [GetSecretValue](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html) 
+  [ListSecrets](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_ListSecrets.html) 
+  [DescribeSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_DescribeSecret.html) 
+  [PutSecretValue](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_PutSecretValue.html) 
+  [RotateSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_RotateSecret.html) 
+  [DeleteSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_DeleteSecret.html) 

For more information about using AWS Secrets Manager, see the [AWS Secrets Manager User Guide](https://docs.aws.amazon.com/secretsmanager/latest/userguide/)\.