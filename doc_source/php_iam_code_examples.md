# IAM examples using SDK for PHP<a name="php_iam_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for PHP with IAM\.

*Actions* are code excerpts that show you how to call individual IAM functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple IAM functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w2aac11c13c13c17c13)
+ [Scenarios](#w2aac11c13c13c17c15)

## Actions<a name="w2aac11c13c13c17c13"></a>

### Attach a policy to a role<a name="iam_AttachRolePolicy_php_topic"></a>

The following code example shows how to attach an IAM policy to a role\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

$assumeRolePolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Principal\": {\"AWS\": \"{$user['Arn']}\"},
                    \"Action\": \"sts:AssumeRole\"
                }]
            }";
$assumeRoleRole = $service->createRole("iam_demo_role_$uuid", $assumeRolePolicyDocument);
echo "Created role: {$assumeRoleRole['RoleName']}\n";

$listAllBucketsPolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Action\": \"s3:ListAllMyBuckets\",
                    \"Resource\": \"arn:aws:s3:::*\"}]
}";
$listAllBucketsPolicy = $service->createPolicy("iam_demo_policy_$uuid", $listAllBucketsPolicyDocument);
echo "Created policy: {$listAllBucketsPolicy['PolicyName']}\n";

$service->attachRolePolicy($assumeRoleRole['RoleName'], $listAllBucketsPolicy['Arn']);

    public function attachRolePolicy($roleName, $policyArn)
    {
        return $this->customWaiter(function () use ($roleName, $policyArn) {
            $this->iamClient->attachRolePolicy([
                'PolicyArn' => $policyArn,
                'RoleName' => $roleName,
            ]);
        });
    }
```
+  For API details, see [AttachRolePolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/AttachRolePolicy) in *AWS SDK for PHP API Reference*\. 

### Create a policy<a name="iam_CreatePolicy_php_topic"></a>

The following code example shows how to create an IAM policy\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

$listAllBucketsPolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Action\": \"s3:ListAllMyBuckets\",
                    \"Resource\": \"arn:aws:s3:::*\"}]
}";
$listAllBucketsPolicy = $service->createPolicy("iam_demo_policy_$uuid", $listAllBucketsPolicyDocument);
echo "Created policy: {$listAllBucketsPolicy['PolicyName']}\n";

    public function createPolicy(string $policyName, string $policyDocument)
    {
        $result = $this->customWaiter(function () use ($policyName, $policyDocument) {
            return $this->iamClient->createPolicy([
                'PolicyName' => $policyName,
                'PolicyDocument' => $policyDocument,
            ]);
        });
        return $result['Policy'];
    }
```
+  For API details, see [CreatePolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreatePolicy) in *AWS SDK for PHP API Reference*\. 

### Create a role<a name="iam_CreateRole_php_topic"></a>

The following code example shows how to create an IAM role\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

$assumeRolePolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Principal\": {\"AWS\": \"{$user['Arn']}\"},
                    \"Action\": \"sts:AssumeRole\"
                }]
            }";
$assumeRoleRole = $service->createRole("iam_demo_role_$uuid", $assumeRolePolicyDocument);
echo "Created role: {$assumeRoleRole['RoleName']}\n";

    /**
     * @param string $roleName
     * @param string $rolePolicyDocument
     * @return array
     * @throws AwsException
     */
    public function createRole(string $roleName, string $rolePolicyDocument)
    {
        $result = $this->customWaiter(function () use ($roleName, $rolePolicyDocument) {
            return $this->iamClient->createRole([
                'AssumeRolePolicyDocument' => $rolePolicyDocument,
                'RoleName' => $roleName,
            ]);
        });
        return $result['Role'];
    }
```
+  For API details, see [CreateRole](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateRole) in *AWS SDK for PHP API Reference*\. 

### Create a service\-linked role<a name="iam_CreateServiceLinkedRole_php_topic"></a>

The following code example shows how to create an IAM service\-linked role\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function createServiceLinkedRole($awsServiceName, $customSuffix = "", $description = "")
    {
        $createServiceLinkedRoleArguments = ['AWSServiceName' => $awsServiceName];
        if ($customSuffix) {
            $createServiceLinkedRoleArguments['CustomSuffix'] = $customSuffix;
        }
        if ($description) {
            $createServiceLinkedRoleArguments['Description'] = $description;
        }
        return $this->iamClient->createServiceLinkedRole($createServiceLinkedRoleArguments);
    }
```
+  For API details, see [CreateServiceLinkedRole](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateServiceLinkedRole) in *AWS SDK for PHP API Reference*\. 

### Create a user<a name="iam_CreateUser_php_topic"></a>

The following code example shows how to create an IAM user\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

$user = $service->createUser("iam_demo_user_$uuid");
echo "Created user with the arn: {$user['Arn']}\n";


    /**
     * @param string $name
     * @return array
     * @throws AwsException
     */
    public function createUser(string $name): array
    {
        $result = $this->iamClient->createUser([
            'UserName' => $name,
        ]);

        return $result['User'];
    }
```
+  For API details, see [CreateUser](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateUser) in *AWS SDK for PHP API Reference*\. 

### Get a policy<a name="iam_GetPolicy_php_topic"></a>

The following code example shows how to get an IAM policy\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function getPolicy($policyArn)
    {
        return $this->customWaiter(function () use ($policyArn) {
            return $this->iamClient->getPolicy(['PolicyArn' => $policyArn]);
        });
    }
```
+  For API details, see [GetPolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/GetPolicy) in *AWS SDK for PHP API Reference*\. 

### Get a role<a name="iam_GetRole_php_topic"></a>

The following code example shows how to get an IAM role\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function getRole($roleName)
    {
        return $this->customWaiter(function () use ($roleName) {
            return $this->iamClient->getRole(['RoleName' => $roleName]);
        });
    }
```
+  For API details, see [GetRole](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/GetRole) in *AWS SDK for PHP API Reference*\. 

### Get the account password policy<a name="iam_GetAccountPasswordPolicy_php_topic"></a>

The following code example shows how to get the IAM account password policy\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function getAccountPasswordPolicy()
    {
        return $this->iamClient->getAccountPasswordPolicy();
    }
```
+  For API details, see [GetAccountPasswordPolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/GetAccountPasswordPolicy) in *AWS SDK for PHP API Reference*\. 

### List SAML providers<a name="iam_ListSAMLProviders_php_topic"></a>

The following code example shows how to list SAML providers for IAM\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function listSAMLProviders()
    {
        return $this->iamClient->listSAMLProviders();
    }
```
+  For API details, see [ListSAMLProviders](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListSAMLProviders) in *AWS SDK for PHP API Reference*\. 

### List groups<a name="iam_ListGroups_php_topic"></a>

The following code example shows how to list IAM groups\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function listGroups($pathPrefix = "", $marker = "", $maxItems = 0)
    {
        $listGroupsArguments = [];
        if ($pathPrefix) {
            $listGroupsArguments["PathPrefix"] = $pathPrefix;
        }
        if ($marker) {
            $listGroupsArguments["Marker"] = $marker;
        }
        if ($maxItems) {
            $listGroupsArguments["MaxItems"] = $maxItems;
        }

        return $this->iamClient->listGroups($listGroupsArguments);
    }
```
+  For API details, see [ListGroups](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListGroups) in *AWS SDK for PHP API Reference*\. 

### List inline policies for a role<a name="iam_ListRolePolicies_php_topic"></a>

The following code example shows how to list inline policies for an IAM role\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function listRolePolicies($roleName, $marker = "", $maxItems = 0)
    {
        $listRolePoliciesArguments = ['RoleName' => $roleName];
        if ($marker) {
            $listRolePoliciesArguments['Marker'] = $marker;
        }
        if ($maxItems) {
            $listRolePoliciesArguments['MaxItems'] = $maxItems;
        }
        return $this->customWaiter(function () use ($listRolePoliciesArguments) {
            return $this->iamClient->listRolePolicies($listRolePoliciesArguments);
        });
    }
```
+  For API details, see [ListRolePolicies](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListRolePolicies) in *AWS SDK for PHP API Reference*\. 

### List policies<a name="iam_ListPolicies_php_topic"></a>

The following code example shows how to list IAM policies\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function listPolicies($pathPrefix = "", $marker = "", $maxItems = 0)
    {
        $listPoliciesArguments = [];
        if ($pathPrefix) {
            $listPoliciesArguments["PathPrefix"] = $pathPrefix;
        }
        if ($marker) {
            $listPoliciesArguments["Marker"] = $marker;
        }
        if ($maxItems) {
            $listPoliciesArguments["MaxItems"] = $maxItems;
        }

        return $this->iamClient->listPolicies($listPoliciesArguments);
    }
```
+  For API details, see [ListPolicies](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListPolicies) in *AWS SDK for PHP API Reference*\. 

### List policies attached to a role<a name="iam_ListAttachedRolePolicies_php_topic"></a>

The following code example shows how to list policies attached to an IAM role\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function listAttachedRolePolicies($roleName, $pathPrefix = "", $marker = "", $maxItems = 0)
    {
        $listAttachRolePoliciesArguments = ['RoleName' => $roleName];
        if ($pathPrefix) {
            $listAttachRolePoliciesArguments['PathPrefix'] = $pathPrefix;
        }
        if ($marker) {
            $listAttachRolePoliciesArguments['Marker'] = $marker;
        }
        if ($maxItems) {
            $listAttachRolePoliciesArguments['MaxItems'] = $maxItems;
        }
        return $this->iamClient->listAttachedRolePolicies($listAttachRolePoliciesArguments);
    }
```
+  For API details, see [ListAttachedRolePolicies](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListAttachedRolePolicies) in *AWS SDK for PHP API Reference*\. 

### List roles<a name="iam_ListRoles_php_topic"></a>

The following code example shows how to list IAM roles\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    /**
     * @param string $pathPrefix
     * @param string $marker
     * @param int $maxItems
     * @return Result
     * $roles = $service->listRoles();
     */
    public function listRoles($pathPrefix = "", $marker = "", $maxItems = 0)
    {
        $listRolesArguments = [];
        if ($pathPrefix) {
            $listRolesArguments["PathPrefix"] = $pathPrefix;
        }
        if ($marker) {
            $listRolesArguments["Marker"] = $marker;
        }
        if ($maxItems) {
            $listRolesArguments["MaxItems"] = $maxItems;
        }
        return $this->iamClient->listRoles($listRolesArguments);
    }
```
+  For API details, see [ListRoles](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListRoles) in *AWS SDK for PHP API Reference*\. 

### List users<a name="iam_ListUsers_php_topic"></a>

The following code example shows how to list IAM users\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
$uuid = uniqid();
$service = new IamService();

    public function listUsers($pathPrefix = "", $marker = "", $maxItems = 0)
    {
        $listUsersArguments = [];
        if ($pathPrefix) {
            $listUsersArguments["PathPrefix"] = $pathPrefix;
        }
        if ($marker) {
            $listUsersArguments["Marker"] = $marker;
        }
        if ($maxItems) {
            $listUsersArguments["MaxItems"] = $maxItems;
        }

        return $this->iamClient->listUsers($listUsersArguments);
    }
```
+  For API details, see [ListUsers](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/ListUsers) in *AWS SDK for PHP API Reference*\. 

## Scenarios<a name="w2aac11c13c13c17c15"></a>

### Create a user and assume a role<a name="iam_Scenario_CreateUserAssumeRole_php_topic"></a>

The following code example shows how to:
+ Create a user who has no permissions\.
+ Create a role that grants permission to list Amazon S3 buckets for the account\.
+ Add a policy to let the user assume the role\.
+ Assume the role and list Amazon S3 buckets using temporary credentials\.
+ Delete the policy, role, and user\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/iam/iam_basics#code-examples)\. 
  

```
namespace Iam\Basics;

require 'vendor/autoload.php';

use Aws\Credentials\Credentials;
use Aws\S3\Exception\S3Exception;
use Aws\S3\S3Client;
use Aws\Sts\StsClient;
use Iam\IamService;

echo("--------------------------------------\n");
print("Welcome to the Amazon IAM getting started demo using PHP!\n");
echo("--------------------------------------\n");
$uuid = uniqid();
$service = new IamService();

$user = $service->createUser("iam_demo_user_$uuid");
echo "Created user with the arn: {$user['Arn']}\n";

$key = $service->createAccessKey($user['UserName']);
$assumeRolePolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Principal\": {\"AWS\": \"{$user['Arn']}\"},
                    \"Action\": \"sts:AssumeRole\"
                }]
            }";
$assumeRoleRole = $service->createRole("iam_demo_role_$uuid", $assumeRolePolicyDocument);
echo "Created role: {$assumeRoleRole['RoleName']}\n";

$listAllBucketsPolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Action\": \"s3:ListAllMyBuckets\",
                    \"Resource\": \"arn:aws:s3:::*\"}]
}";
$listAllBucketsPolicy = $service->createPolicy("iam_demo_policy_$uuid", $listAllBucketsPolicyDocument);
echo "Created policy: {$listAllBucketsPolicy['PolicyName']}\n";

$service->attachRolePolicy($assumeRoleRole['RoleName'], $listAllBucketsPolicy['Arn']);

$inlinePolicyDocument = "{
                \"Version\": \"2012-10-17\",
                \"Statement\": [{
                    \"Effect\": \"Allow\",
                    \"Action\": \"sts:AssumeRole\",
                    \"Resource\": \"{$assumeRoleRole['Arn']}\"}]
}";
$inlinePolicy = $service->createUserPolicy("iam_demo_inline_policy_$uuid", $inlinePolicyDocument, $user['UserName']);
//First, fail to list the buckets with the user
$credentials = new Credentials($key['AccessKeyId'], $key['SecretAccessKey']);
$s3Client = new S3Client(['region' => 'us-west-2', 'version' => 'latest', 'credentials' => $credentials]);
try {
    $s3Client->listBuckets([
    ]);
    echo "this should not run";
} catch (S3Exception $exception) {
    echo "successfully failed!\n";
}

$stsClient = new StsClient(['region' => 'us-west-2', 'version' => 'latest', 'credentials' => $credentials]);
sleep(10);
$assumedRole = $stsClient->assumeRole([
    'RoleArn' => $assumeRoleRole['Arn'],
    'RoleSessionName' => "DemoAssumeRoleSession_$uuid",
]);
$assumedCredentials = [
    'key' => $assumedRole['Credentials']['AccessKeyId'],
    'secret' => $assumedRole['Credentials']['SecretAccessKey'],
    'token' => $assumedRole['Credentials']['SessionToken'],
];
$s3Client = new S3Client(['region' => 'us-west-2', 'version' => 'latest', 'credentials' => $assumedCredentials]);
try {
    $s3Client->listBuckets([
    ]);
    echo "this should now run!\n";
} catch (S3Exception $exception) {
    echo "this should now not fail\n";
}

$service->detachRolePolicy($assumeRoleRole['RoleName'], $listAllBucketsPolicy['Arn']);
$deletePolicy = $service->deletePolicy($listAllBucketsPolicy['Arn']);
echo "Delete policy: {$listAllBucketsPolicy['PolicyName']}\n";
$deletedRole = $service->deleteRole($assumeRoleRole['Arn']);
echo "Deleted role: {$assumeRoleRole['RoleName']}\n";
$deletedKey = $service->deleteAccessKey($key['AccessKeyId']);
$deletedUser = $service->deleteUser($user['UserName']);
echo "Delete user: {$user['UserName']}\n";
```
+ For API details, see the following topics in *AWS SDK for PHP API Reference*\.
  + [AttachRolePolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/AttachRolePolicy)
  + [CreateAccessKey](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateAccessKey)
  + [CreatePolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreatePolicy)
  + [CreateRole](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateRole)
  + [CreateUser](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/CreateUser)
  + [DeleteAccessKey](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/DeleteAccessKey)
  + [DeletePolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/DeletePolicy)
  + [DeleteRole](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/DeleteRole)
  + [DeleteUser](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/DeleteUser)
  + [DeleteUserPolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/DeleteUserPolicy)
  + [DetachRolePolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/DetachRolePolicy)
  + [PutUserPolicy](https://docs.aws.amazon.com/goto/SdkForPHPV3/iam-2010-05-08/PutUserPolicy)