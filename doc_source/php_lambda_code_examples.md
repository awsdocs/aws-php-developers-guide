# Lambda examples using SDK for PHP<a name="php_lambda_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for PHP with Lambda\.

*Actions* are code excerpts that show you how to call individual Lambda functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Lambda functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w2aac11c13c13c19c13)
+ [Scenarios](#w2aac11c13c13c19c15)

## Actions<a name="w2aac11c13c13c19c13"></a>

### Create a function<a name="lambda_CreateFunction_php_topic"></a>

The following code example shows how to create a Lambda function\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function createFunction($functionName, $role, $bucketName, $handler)
    {
        //This assumes the Lambda function is in an S3 bucket.
        return $this->customWaiter(function () use ($functionName, $role, $bucketName, $handler) {
            return $this->lambdaClient->createFunction([
                'Code' => [
                    'S3Bucket' => $bucketName,
                    'S3Key' => $functionName,
                ],
                'FunctionName' => $functionName,
                'Role' => $role['Arn'],
                'Runtime' => 'python3.9',
                'Handler' => "$handler.lambda_handler",
            ]);
        });
    }
```
+  For API details, see [CreateFunction](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/CreateFunction) in *AWS SDK for PHP API Reference*\. 

### Delete a function<a name="lambda_DeleteFunction_php_topic"></a>

The following code example shows how to delete a Lambda function\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function deleteFunction($functionName)
    {
        return $this->lambdaClient->deleteFunction([
            'FunctionName' => $functionName,
        ]);
    }
```
+  For API details, see [DeleteFunction](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/DeleteFunction) in *AWS SDK for PHP API Reference*\. 

### Get a function<a name="lambda_GetFunction_php_topic"></a>

The following code example shows how to get a Lambda function\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function getFunction($functionName)
    {
        return $this->lambdaClient->getFunction([
            'FunctionName' => $functionName,
        ]);
    }
```
+  For API details, see [GetFunction](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/GetFunction) in *AWS SDK for PHP API Reference*\. 

### Invoke a function<a name="lambda_Invoke_php_topic"></a>

The following code example shows how to invoke a Lambda function\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function invoke($functionName, $params, $logType = 'None')
    {
        return $this->lambdaClient->invoke([
            'FunctionName' => $functionName,
            'Payload' => json_encode($params),
            'LogType' => $logType,
        ]);
    }
```
+  For API details, see [Invoke](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/Invoke) in *AWS SDK for PHP API Reference*\. 

### List functions<a name="lambda_ListFunctions_php_topic"></a>

The following code example shows how to list Lambda functions\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function listFunctions($maxItems = 50, $marker = null)
    {
        if (is_null($marker)) {
            return $this->lambdaClient->listFunctions([
                'MaxItems' => $maxItems,
            ]);
        }

        return $this->lambdaClient->listFunctions([
            'Marker' => $marker,
            'MaxItems' => $maxItems,
        ]);
    }
```
+  For API details, see [ListFunctions](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/ListFunctions) in *AWS SDK for PHP API Reference*\. 

### Update function code<a name="lambda_UpdateFunctionCode_php_topic"></a>

The following code example shows how to update Lambda function code\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function updateFunctionCode($functionName, $s3Bucket, $s3Key)
    {
        return $this->lambdaClient->updateFunctionCode([
            'FunctionName' => $functionName,
            'S3Bucket' => $s3Bucket,
            'S3Key' => $s3Key,
        ]);
    }
```
+  For API details, see [UpdateFunctionCode](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/UpdateFunctionCode) in *AWS SDK for PHP API Reference*\. 

### Update function configuration<a name="lambda_UpdateFunctionConfiguration_php_topic"></a>

The following code example shows how to update Lambda function configuration\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
    public function updateFunctionConfiguration($functionName, $handler, $environment = '')
    {
        return $this->lambdaClient->updateFunctionConfiguration([
            'FunctionName' => $functionName,
            'Handler' => "$handler.lambda_handler",
            'Environment' => $environment,
        ]);
    }
```
+  For API details, see [UpdateFunctionConfiguration](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/UpdateFunctionConfiguration) in *AWS SDK for PHP API Reference*\. 

## Scenarios<a name="w2aac11c13c13c19c15"></a>

### Get started with functions<a name="lambda_Scenario_GettingStartedFunctions_php_topic"></a>

The following code example shows how to:
+ Create an AWS Identity and Access Management \(IAM\) role that grants Lambda permission to write to logs\.
+ Create a Lambda function and upload handler code\.
+ Invoke the function with a single parameter and get results\.
+ Update the function code and configure its Lambda environment with an environment variable\.
+ Invoke the function with new parameters and get results\. Display the execution log that's returned from the invocation\.
+ List the functions for your account\.
+ Delete the IAM role and the Lambda function\.

For more information, see [Create a Lambda function with the console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/lambda#code-examples)\. 
  

```
namespace Lambda;

use Aws\S3\S3Client;
use GuzzleHttp\Psr7\Stream;
use Iam\IamService;

class GettingStartedWithLambda
{
    public function run()
    {
        echo("--------------------------------------\n");
        print("Welcome to the AWS Lambda getting started demo using PHP!\n");
        echo("--------------------------------------\n");

        $clientArgs = [
            'region' => 'us-west-2',
            'version' => 'latest',
            'profile' => 'default',
        ];
        $uniqid = uniqid();

        $iamService = new IamService();
        $s3client = new S3Client($clientArgs);
        $lambdaService = new LambdaService();

        echo "First, let's create a role to run our Lambda code.\n";
        $roleName = "test-lambda-role-$uniqid";
        $rolePolicyDocument = "{
            \"Version\": \"2012-10-17\",
            \"Statement\": [
                {
                    \"Effect\": \"Allow\",
                    \"Principal\": {
                        \"Service\": \"lambda.amazonaws.com\"
                    },
                    \"Action\": \"sts:AssumeRole\"
                }
            ]
        }";
        $role = $iamService->createRole($roleName, $rolePolicyDocument);
        echo "Created role {$role['RoleName']}.\n";

        $iamService->attachRolePolicy(
            $role['RoleName'],
            "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
        );
        echo "Attached the AWSLambdaBasicExecutionRole to {$role['RoleName']}.\n";

        echo "\nNow let's create an S3 bucket and upload our Lambda code there.\n";
        $bucketName = "test-example-bucket-$uniqid";
        $s3client->createBucket([
            'Bucket' => $bucketName,
        ]);
        echo "Created bucket $bucketName.\n";

        $functionName = "doc_example_lambda_$uniqid";
        $codeBasic = "lambda_handler_basic.zip";
        $handler = "lambda_handler_basic";
        $file = file_get_contents($codeBasic);
        $s3client->putObject([
            'Bucket' => $bucketName,
            'Key' => $functionName,
            'Body' => $file,
        ]);
        echo "Uploaded the Lambda code.\n";

        $createLambdaFunction = $lambdaService->createFunction($functionName, $role, $bucketName, $handler);
        // Wait until the function has finished being created.
        do {
            $getLambdaFunction = $lambdaService->getFunction($createLambdaFunction['FunctionName']);
        } while ($getLambdaFunction['Configuration']['State'] == "Pending");
        echo "Created Lambda function {$getLambdaFunction['Configuration']['FunctionName']}.\n";

        sleep(1);

        echo "\nOk, let's invoke that Lambda code.\n";
        $basicParams = [
            'action' => 'increment',
            'number' => 3,
        ];
        /** @var Stream $invokeFunction */
        $invokeFunction = $lambdaService->invoke($functionName, $basicParams)['Payload'];
        $result = json_decode($invokeFunction->getContents())->result;
        echo "After invoking the Lambda code with the input of {$basicParams['number']} we received $result.\n";

        echo "\nSince that's working, let's update the Lambda code.\n";
        $codeCalculator = "lambda_handler_calculator.zip";
        $handlerCalculator = "lambda_handler_calculator";
        echo "First, put the new code into the S3 bucket.\n";
        $file = file_get_contents($codeCalculator);
        $s3client->putObject([
            'Bucket' => $bucketName,
            'Key' => $functionName,
            'Body' => $file,
        ]);
        echo "New code uploaded.\n";

        $lambdaService->updateFunctionCode($functionName, $bucketName, $functionName);
        // Wait for the Lambda code to finish updating.
        do {
            $getLambdaFunction = $lambdaService->getFunction($createLambdaFunction['FunctionName']);
        } while ($getLambdaFunction['Configuration']['LastUpdateStatus'] !== "Successful");
        echo "New Lambda code uploaded.\n";

        $environment = [
            'Variable' => ['Variables' => ['LOG_LEVEL' => 'DEBUG']],
        ];
        $lambdaService->updateFunctionConfiguration($functionName, $handlerCalculator, $environment);
        do {
            $getLambdaFunction = $lambdaService->getFunction($createLambdaFunction['FunctionName']);
        } while ($getLambdaFunction['Configuration']['LastUpdateStatus'] !== "Successful");
        echo "Lambda code updated with new handler and a LOG_LEVEL of DEBUG for more information.\n";

        echo "Invoke the new code with some new data.\n";
        $calculatorParams = [
            'action' => 'plus',
            'x' => 5,
            'y' => 4,
        ];
        $invokeFunction = $lambdaService->invoke($functionName, $calculatorParams, "Tail");
        $result = json_decode($invokeFunction['Payload']->getContents())->result;
        echo "Indeed, {$calculatorParams['x']} + {$calculatorParams['y']} does equal $result.\n";
        echo "Here's the extra debug info: ";
        echo base64_decode($invokeFunction['LogResult']) . "\n";

        echo "\nBut what happens if you try to divide by zero?\n";
        $divZeroParams = [
            'action' => 'divide',
            'x' => 5,
            'y' => 0,
        ];
        $invokeFunction = $lambdaService->invoke($functionName, $divZeroParams, "Tail");
        $result = json_decode($invokeFunction['Payload']->getContents())->result;
        echo "You get a |$result| result.\n";
        echo "And an error message: ";
        echo base64_decode($invokeFunction['LogResult']) . "\n";

        echo "\nHere's all the Lambda functions you have in this Region:\n";
        $listLambdaFunctions = $lambdaService->listFunctions(5);
        $allLambdaFunctions = $listLambdaFunctions['Functions'];
        $next = $listLambdaFunctions->get('NextMarker');
        while ($next != false) {
            $listLambdaFunctions = $lambdaService->listFunctions(5, $next);
            $next = $listLambdaFunctions->get('NextMarker');
            $allLambdaFunctions = array_merge($allLambdaFunctions, $listLambdaFunctions['Functions']);
        }
        foreach ($allLambdaFunctions as $function) {
            echo "{$function['FunctionName']}\n";
        }

        echo "\n\nAnd don't forget to clean up your data!\n";

        $lambdaService->deleteFunction($functionName);
        echo "Deleted Lambda function.\n";
        $iamService->deleteRole($role['RoleName']);
        echo "Deleted Role.\n";
        $deleteObjects = $s3client->listObjectsV2([
            'Bucket' => $bucketName,
        ]);
        $deleteObjects = $s3client->deleteObjects([
            'Bucket' => $bucketName,
            'Delete' => [
                'Objects' => $deleteObjects['Contents'],
            ]
        ]);
        echo "Deleted all objects from the S3 bucket.\n";
        $s3client->deleteBucket(['Bucket' => $bucketName]);
        echo "Deleted the bucket.\n";
    }
}
```
+ For API details, see the following topics in *AWS SDK for PHP API Reference*\.
  + [CreateFunction](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/CreateFunction)
  + [DeleteFunction](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/DeleteFunction)
  + [GetFunction](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/GetFunction)
  + [Invoke](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/Invoke)
  + [ListFunctions](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/ListFunctions)
  + [UpdateFunctionCode](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/UpdateFunctionCode)
  + [UpdateFunctionConfiguration](https://docs.aws.amazon.com/goto/SdkForPHPV3/lambda-2015-03-31/UpdateFunctionConfiguration)