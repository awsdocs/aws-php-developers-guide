# Authorize SDK Metrics to Collect and Send Metrics in the AWS SDK for PHP Version 3<a name="guide_sdk-metrics-set-permissions"></a>

To collect metrics from AWS SDKs using AWS SDK Metrics for Enterprise Support Enterprise customers must create an IAM Role that gives CloudWatch agent permission to gather data from their Amazon EC2 instance or production environment\.

Use the following PHP code sample or the AWS Console to create an IAM Policy and Role for an CloudWatch agent to access SDK Metrics in your environment\.

Learn more about using SDK Metrics with AWS SDK for PHP in [Set Up SDK Metrics for the AWS SDK for PHP Version 3](guide_sdk-metrics-configure.md)\. For more information about SDK Metrics see [IAM Permissions for SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Set-IAM-Permissions-For-SDK-Metrics.html) in the Amazon CloudWatch User Guide\.

## Set Up Access Permissions Using the AWS SDK for PHP<a name="set-up-access-permissions-using-the-sdk-php"></a>

Create an IAM role for the instance that has permission for Amazon EC2 Systems Manager and SDK Metrics\.

First, create a policy using [CreatePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#createpolicy)\. Then create a role using [CreateRole](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#createrole)\. Finally, attach the policy you created to your new role with [AttachRolePolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-iam-2010-05-08.html#attachrolepolicy)\.

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

$roleName = 'AmazonCSM';

$description = 'An Instance role that has permission for  Amazon EC2 Systems Manager  and SDK Metric Monitoring.';

$AmazonCSMPolicy = '{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "sdkmetrics-beta:*"
                ],
                "Resource": "*"
            },
            {   
                "Effect": "Allow",
                "Action": [
                    "ssm:GetParameter"
                ],
                "Resource": "arn:aws:ssm:*:*:parameter/AmazonCSM*"
            }
        ]
    }';

$rolePolicy = '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}';


try {
    $iamPolicy = $client->createPolicy([
        'PolicyName' => $roleName . 'policy',
        'PolicyDocument' => $AmazonCSMPolicy
    ]);
    if ($iamPolicy['@metadata']['statusCode'] == 200) {
        $policyArn = $iamPolicy['Policy']['Arn'];
        echo('<p> Your IAM Policy has been created. Arn -  ');
        echo($policyArn);
        echo('<p>');
        $role = $client->createRole([
            'RoleName' => $roleName,
            'Description' => $description,
            'AssumeRolePolicyDocument' => $rolePolicy,
        ]);
        echo('<p> Your IAM User Role has been created. Arn: ');
        echo($role['Role']['Arn']);
        echo('<p>');
        if ($role['@metadata']['statusCode'] == 200) {
            $result = $client->attachRolePolicy([
                'PolicyArn' => $policyArn,
                'RoleName' => $roleName,
            ]);
            var_dump($result)
        } else {
            echo('<p> There was an error creating your IAM User Role </p>');
            var_dump($role);
        }
    } else {
        echo('<p> There was an error creating your IAM Policy </p>');
        var_dump($iamPolicy);
        
    }
} catch (AwsException $e) {
    // output error message if fails
    echo $e;
    error_log($e->getMessage());
    
}
```

## Set Up Access Permissions by Using the IAM Console<a name="set-up-access-permissions-by-using-the-iam-console"></a>

Alternatively, you can use the IAM console to create a role\.

1. Go to the [IAM console](https://console.aws.amazon.com/iam), and create a role to use Amazon EC2\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create Role**\.

1. Choose **AWS Service**, and then choose **EC2**\.

1. Choose **Next: Permissions**\.

1. Under **Attach permissions policies**, choose **create policy**\.

1. For **Service**, choose **Systems Manager**\. For **Actions**, expand `Read`, and choose `GetParameters`\. For resources, specify your CloudWatch agent\.

1. Add additional permission\.

1. Select **Choose a service**, and then **Enter service manually**\. For **Service**, enter `sdkmetrics`\. Select all `sdkmetrics` actions and all resources, and then choose **Review Policy**\.

1. Name the **Role** `AmazonSDKMetrics`, and add a description\.

1. Choose **Create Role**\.