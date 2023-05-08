# Auto Scaling examples using SDK for PHP<a name="php_auto-scaling_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for PHP with Auto Scaling\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Get started**

## Hello Auto Scaling<a name="example_auto-scaling_Hello_section"></a>

The following code examples show how to get started using Auto Scaling\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function helloService()
    {
        $autoScalingClient = new AutoScalingClient([
            'region' => 'us-west-2',
            'version' => 'latest',
            'profile' => 'default',
        ]);

        $groups = $autoScalingClient->describeAutoScalingGroups([]);
        var_dump($groups);
    }
```
+  For API details, see [DescribeAutoScalingGroups](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeAutoScalingGroups) in *AWS SDK for PHP API Reference*\. 

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Create a group<a name="auto-scaling_CreateAutoScalingGroup_php_topic"></a>

The following code example shows how to create an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function createAutoScalingGroup(
        $autoScalingGroupName,
        $availabilityZones,
        $minSize,
        $maxSize,
        $launchTemplateId
    ) {
        return $this->autoScalingClient->createAutoScalingGroup([
            'AutoScalingGroupName' => $autoScalingGroupName,
            'AvailabilityZones' => $availabilityZones,
            'MinSize' => $minSize,
            'MaxSize' => $maxSize,
            'LaunchTemplate' => [
                'LaunchTemplateId' => $launchTemplateId,
            ],
        ]);
    }
```
+  For API details, see [CreateAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/CreateAutoScalingGroup) in *AWS SDK for PHP API Reference*\. 

### Delete a group<a name="auto-scaling_DeleteAutoScalingGroup_php_topic"></a>

The following code example shows how to delete an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function deleteAutoScalingGroup($autoScalingGroupName)
    {
        return $this->autoScalingClient->deleteAutoScalingGroup([
            'AutoScalingGroupName' => $autoScalingGroupName,
            'ForceDelete' => true,
        ]);
    }
```
+  For API details, see [DeleteAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DeleteAutoScalingGroup) in *AWS SDK for PHP API Reference*\. 

### Disable metrics collection for a group<a name="auto-scaling_DisableMetricsCollection_php_topic"></a>

The following code example shows how to disable CloudWatch metrics collection for an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function disableMetricsCollection($autoScalingGroupName)
    {
        return $this->autoScalingClient->disableMetricsCollection([
            'AutoScalingGroupName' => $autoScalingGroupName,
        ]);
    }
```
+  For API details, see [DisableMetricsCollection](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DisableMetricsCollection) in *AWS SDK for PHP API Reference*\. 

### Enable metrics collection for a group<a name="auto-scaling_EnableMetricsCollection_php_topic"></a>

The following code example shows how to enable CloudWatch metrics collection for an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function enableMetricsCollection($autoScalingGroupName, $granularity)
    {
        return $this->autoScalingClient->enableMetricsCollection([
            'AutoScalingGroupName' => $autoScalingGroupName,
            'Granularity' => $granularity,
        ]);
    }
```
+  For API details, see [EnableMetricsCollection](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/EnableMetricsCollection) in *AWS SDK for PHP API Reference*\. 

### Get information about groups<a name="auto-scaling_DescribeAutoScalingGroups_php_topic"></a>

The following code example shows how to get information about Auto Scaling groups\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function describeAutoScalingGroups($autoScalingGroupNames)
    {
        return $this->autoScalingClient->describeAutoScalingGroups([
            'AutoScalingGroupNames' => $autoScalingGroupNames
        ]);
    }
```
+  For API details, see [DescribeAutoScalingGroups](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeAutoScalingGroups) in *AWS SDK for PHP API Reference*\. 

### Get information about instances<a name="auto-scaling_DescribeAutoScalingInstances_php_topic"></a>

The following code example shows how to get information about Auto Scaling instances\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function describeAutoScalingInstances($instanceIds)
    {
        return $this->autoScalingClient->describeAutoScalingInstances([
            'InstanceIds' => $instanceIds
        ]);
    }
```
+  For API details, see [DescribeAutoScalingInstances](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeAutoScalingInstances) in *AWS SDK for PHP API Reference*\. 

### Get information about scaling activities<a name="auto-scaling_DescribeScalingActivities_php_topic"></a>

The following code example shows how to get information about Auto Scaling activities\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function describeScalingActivities($autoScalingGroupName)
    {
        return $this->autoScalingClient->describeScalingActivities([
            'AutoScalingGroupName' => $autoScalingGroupName,
        ]);
    }
```
+  For API details, see [DescribeScalingActivities](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeScalingActivities) in *AWS SDK for PHP API Reference*\. 

### Set the desired capacity of a group<a name="auto-scaling_SetDesiredCapacity_php_topic"></a>

The following code example shows how to set the desired capacity of an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function setDesiredCapacity($autoScalingGroupName, $desiredCapacity)
    {
        return $this->autoScalingClient->setDesiredCapacity([
            'AutoScalingGroupName' => $autoScalingGroupName,
            'DesiredCapacity' => $desiredCapacity,
        ]);
    }
```
+  For API details, see [SetDesiredCapacity](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/SetDesiredCapacity) in *AWS SDK for PHP API Reference*\. 

### Terminate an instance in a group<a name="auto-scaling_TerminateInstanceInAutoScalingGroup_php_topic"></a>

The following code example shows how to terminate an instance in an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function terminateInstanceInAutoScalingGroup(
        $instanceId,
        $shouldDecrementDesiredCapacity = true,
        $attempts = 0
    ) {
        try {
            return $this->autoScalingClient->terminateInstanceInAutoScalingGroup([
                'InstanceId' => $instanceId,
                'ShouldDecrementDesiredCapacity' => $shouldDecrementDesiredCapacity,
            ]);
        } catch (AutoScalingException $exception) {
            if ($exception->getAwsErrorCode() == "ScalingActivityInProgress" && $attempts < 5) {
                error_log("Cannot terminate an instance while it is still pending. Waiting then trying again.");
                sleep(5 * (1 + $attempts));
                return $this->terminateInstanceInAutoScalingGroup(
                    $instanceId,
                    $shouldDecrementDesiredCapacity,
                    ++$attempts
                );
            } else {
                throw $exception;
            }
        }
    }
```
+  For API details, see [TerminateInstanceInAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/TerminateInstanceInAutoScalingGroup) in *AWS SDK for PHP API Reference*\. 

### Update a group<a name="auto-scaling_UpdateAutoScalingGroup_php_topic"></a>

The following code example shows how to update the configuration for an Auto Scaling group\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
    public function updateAutoScalingGroup($autoScalingGroupName, $args)
    {
        if (array_key_exists('MaxSize', $args)) {
            $maxSize = ['MaxSize' => $args['MaxSize']];
        } else {
            $maxSize = [];
        }
        if (array_key_exists('MinSize', $args)) {
            $minSize = ['MinSize' => $args['MinSize']];
        } else {
            $minSize = [];
        }
        $parameters = ['AutoScalingGroupName' => $autoScalingGroupName];
        $parameters = array_merge($parameters, $minSize, $maxSize);
        return $this->autoScalingClient->updateAutoScalingGroup($parameters);
    }
```
+  For API details, see [UpdateAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/UpdateAutoScalingGroup) in *AWS SDK for PHP API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Manage groups and instances<a name="auto-scaling_Scenario_GroupsAndInstances_php_topic"></a>

The following code example shows how to:
+ Create an Amazon EC2 Auto Scaling group with a launch template and Availability Zones, and get information about running instances\.
+ Enable Amazon CloudWatch metrics collection\.
+ Update the group's desired capacity and wait for an instance to start\.
+ Terminate an instance in the group\.
+ List scaling activities that occur in response to user requests and capacity changes\.
+ Get statistics for CloudWatch metrics, then clean up resources\.

**SDK for PHP**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/auto-scaling#code-examples)\. 
  

```
namespace AutoScaling;

use Aws\AutoScaling\AutoScalingClient;
use Aws\CloudWatch\CloudWatchClient;
use Aws\Ec2\Ec2Client;
use AwsUtilities\AWSServiceClass;
use AwsUtilities\RunnableExample;

class GettingStartedWithAutoScaling implements RunnableExample
{
    protected Ec2Client $ec2Client;
    protected AutoScalingClient $autoScalingClient;
    protected AutoScalingService $autoScalingService;
    protected CloudWatchClient $cloudWatchClient;
    protected string $templateName;
    protected string $autoScalingGroupName;
    protected array $role;

    public function runExample()
    {
        echo("--------------------------------------\n");
        print("Welcome to the Amazon EC2 Auto Scaling getting started demo using PHP!\n");
        echo("--------------------------------------\n");

        $clientArgs = [
            'region' => 'us-west-2',
            'version' => 'latest',
            'profile' => 'default',
        ];
        $uniqid = uniqid();

        $this->autoScalingClient = new AutoScalingClient($clientArgs);
        $this->autoScalingService = new AutoScalingService($this->autoScalingClient);
        $this->cloudWatchClient = new CloudWatchClient($clientArgs);

        AWSServiceClass::$waitTime = 5;
        AWSServiceClass::$maxWaitAttempts = 20;

        /**
         * Step 0: Create an EC2 launch template that you'll use to create an Auto Scaling group.
         */
        $this->ec2Client = new EC2Client($clientArgs);
        $this->templateName = "example_launch_template_$uniqid";
        $instanceType = "t1.micro";
        $amiId = "ami-0ca285d4c2cda3300";
        $launchTemplate = $this->ec2Client->createLaunchTemplate([
            'LaunchTemplateName' => $this->templateName,
            'LaunchTemplateData' => [
                'InstanceType' => $instanceType,
                'ImageId' => $amiId,
            ]
        ]);

        /**
         * Step 1: CreateAutoScalingGroup: pass it the launch template you created in step 0.
         */
        $availabilityZones[] = $this->ec2Client->describeAvailabilityZones([])['AvailabilityZones'][1]['ZoneName'];

        $this->autoScalingGroupName = "demoAutoScalingGroupName_$uniqid";
        $minSize = 1;
        $maxSize = 1;
        $launchTemplateId = $launchTemplate['LaunchTemplate']['LaunchTemplateId'];
        $this->autoScalingService->createAutoScalingGroup(
            $this->autoScalingGroupName,
            $availabilityZones,
            $minSize,
            $maxSize,
            $launchTemplateId
        );

        $this->autoScalingService->waitUntilGroupInService([$this->autoScalingGroupName]);
        $autoScalingGroup = $this->autoScalingService->describeAutoScalingGroups([$this->autoScalingGroupName]);

        /**
         * Step 2: DescribeAutoScalingInstances: show that one instance has launched.
         */
        $instanceIds = [$autoScalingGroup['AutoScalingGroups'][0]['Instances'][0]['InstanceId']];
        $instances = $this->autoScalingService->describeAutoScalingInstances($instanceIds);
        echo "The Auto Scaling group {$this->autoScalingGroupName} was created successfully.\n";
        echo count($instances['AutoScalingInstances']) . " instances were created for the group.\n";
        echo $autoScalingGroup['AutoScalingGroups'][0]['MaxSize'] . " is the max number of instances for the group.\n";

        /**
         * Step 3: EnableMetricsCollection: enable all metrics or a subset.
         */
        $this->autoScalingService->enableMetricsCollection($this->autoScalingGroupName, "1Minute");

        /**
         * Step 4: UpdateAutoScalingGroup: update max size to 3.
         */
        echo "Updating the max number of instances to 3.\n";
        $this->autoScalingService->updateAutoScalingGroup($this->autoScalingGroupName, ['MaxSize' => 3]);

        /**
         * Step 5: DescribeAutoScalingGroups: show the current state of the group.
         */
        $autoScalingGroup = $this->autoScalingService->describeAutoScalingGroups([$this->autoScalingGroupName]);
        echo $autoScalingGroup['AutoScalingGroups'][0]['MaxSize'];
        echo " is the updated max number of instances for the group.\n";

        $limits = $this->autoScalingService->describeAccountLimits();
        echo "Here are your account limits:\n";
        echo "MaxNumberOfAutoScalingGroups: {$limits['MaxNumberOfAutoScalingGroups']}\n";
        echo "MaxNumberOfLaunchConfigurations: {$limits['MaxNumberOfLaunchConfigurations']}\n";
        echo "NumberOfAutoScalingGroups: {$limits['NumberOfAutoScalingGroups']}\n";
        echo "NumberOfLaunchConfigurations: {$limits['NumberOfLaunchConfigurations']}\n";

        /**
         * Step 6: SetDesiredCapacity: set desired capacity to 2.
         */
        $this->autoScalingService->setDesiredCapacity($this->autoScalingGroupName, 2);
        sleep(10); // Wait for the group to start processing the request.
        $this->autoScalingService->waitUntilGroupInService([$this->autoScalingGroupName]);

        /**
         * Step 7: DescribeAutoScalingInstances: show that two instances are launched.
         */
        $autoScalingGroups = $this->autoScalingService->describeAutoScalingGroups([$this->autoScalingGroupName]);
        foreach ($autoScalingGroups['AutoScalingGroups'] as $autoScalingGroup) {
            echo "There is a group named: {$autoScalingGroup['AutoScalingGroupName']}";
            echo "with an ARN of {$autoScalingGroup['AutoScalingGroupARN']}.\n";
            foreach ($autoScalingGroup['Instances'] as $instance) {
                echo "{$autoScalingGroup['AutoScalingGroupName']} has an instance with id of: ";
                echo "{$instance['InstanceId']} and a lifecycle state of: {$instance['LifecycleState']}.\n";
            }
        }

        /**
         * Step 8: TerminateInstanceInAutoScalingGroup: terminate one of the instances in the group.
         */
        $this->autoScalingService->terminateInstanceInAutoScalingGroup($instance['InstanceId'], false);
        do {
            sleep(10);
            $instances = $this->autoScalingService->describeAutoScalingInstances([$instance['InstanceId']]);
        } while (count($instances['AutoScalingInstances']) > 0);
        do {
            sleep(10);
            $autoScalingGroups = $this->autoScalingService->describeAutoScalingGroups([$this->autoScalingGroupName]);
            $instances = $autoScalingGroups['AutoScalingGroups'][0]['Instances'];
        } while (count($instances) < 2);
        $this->autoScalingService->waitUntilGroupInService([$this->autoScalingGroupName]);
        foreach ($autoScalingGroups['AutoScalingGroups'] as $autoScalingGroup) {
            echo "There is a group named: {$autoScalingGroup['AutoScalingGroupName']}";
            echo "with an ARN of {$autoScalingGroup['AutoScalingGroupARN']}.\n";
            foreach ($autoScalingGroup['Instances'] as $instance) {
                echo "{$autoScalingGroup['AutoScalingGroupName']} has an instance with id of: ";
                echo "{$instance['InstanceId']} and a lifecycle state of: {$instance['LifecycleState']}.\n";
            }
        }

        /**
         * Step 9: DescribeScalingActivities: list the scaling activities that have occurred for the group so far.
         */
        $activities = $this->autoScalingService->describeScalingActivities($autoScalingGroup['AutoScalingGroupName']);
        echo "We found " . count($activities['Activities']) . " activities.\n";
        foreach ($activities['Activities'] as $activity) {
            echo "{$activity['ActivityId']} - {$activity['StartTime']} - {$activity['Description']}\n";
        }

        /**
         * Step 10: Use the Amazon CloudWatch API to get and show some metrics collected for the group.
         */
        $metricsNamespace = 'AWS/AutoScaling';
        $metricsDimensions = [
            [
                'Name' => 'AutoScalingGroupName',
                'Value' => $autoScalingGroup['AutoScalingGroupName'],
            ],
        ];
        $metrics = $this->cloudWatchClient->listMetrics([
            'Dimensions' => $metricsDimensions,
            'Namespace' => $metricsNamespace,
        ]);
        foreach ($metrics['Metrics'] as $metric) {
            $timespan = 5;
            if ($metric['MetricName'] != 'GroupTotalCapacity' && $metric['MetricName'] != 'GroupMaxSize') {
                continue;
            }
            echo "Over the last $timespan minutes, {$metric['MetricName']} recorded:\n";
            $stats = $this->cloudWatchClient->getMetricStatistics([
                'Dimensions' => $metricsDimensions,
                'EndTime' => time(),
                'StartTime' => time() - (5 * 60),
                'MetricName' => $metric['MetricName'],
                'Namespace' => $metricsNamespace,
                'Period' => 60,
                'Statistics' => ['Sum'],
            ]);
            foreach ($stats['Datapoints'] as $stat) {
                echo "{$stat['Timestamp']}: {$stat['Sum']}\n";
            }
        }

        return $instances;
    }

    public function cleanUp()
    {
        /**
         * Step 11: DisableMetricsCollection: disable all metrics.
         */
        $this->autoScalingService->disableMetricsCollection($this->autoScalingGroupName);

        /**
         * Step 12: DeleteAutoScalingGroup: to delete the group you must stop all instances.
         * - UpdateAutoScalingGroup with MinSize=0
         * - TerminateInstanceInAutoScalingGroup for each instance,
         *     specify ShouldDecrementDesiredCapacity=True. Wait for instances to stop.
         * - Now you can delete the group.
         */
        $this->autoScalingService->updateAutoScalingGroup($this->autoScalingGroupName, ['MinSize' => 0]);
        $this->autoScalingService->terminateAllInstancesInAutoScalingGroup($this->autoScalingGroupName);
        $this->autoScalingService->waitUntilGroupInService([$this->autoScalingGroupName]);
        $this->autoScalingService->deleteAutoScalingGroup($this->autoScalingGroupName);

        /**
         * Step 13: Delete launch template.
         */
        $this->ec2Client->deleteLaunchTemplate([
            'LaunchTemplateName' => $this->templateName,
        ]);
    }

    public function helloService()
    {
        $autoScalingClient = new AutoScalingClient([
            'region' => 'us-west-2',
            'version' => 'latest',
            'profile' => 'default',
        ]);

        $groups = $autoScalingClient->describeAutoScalingGroups([]);
        var_dump($groups);
    }
}
```
+ For API details, see the following topics in *AWS SDK for PHP API Reference*\.
  + [CreateAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/CreateAutoScalingGroup)
  + [DeleteAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DeleteAutoScalingGroup)
  + [DescribeAutoScalingGroups](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeAutoScalingGroups)
  + [DescribeAutoScalingInstances](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeAutoScalingInstances)
  + [DescribeScalingActivities](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DescribeScalingActivities)
  + [DisableMetricsCollection](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/DisableMetricsCollection)
  + [EnableMetricsCollection](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/EnableMetricsCollection)
  + [SetDesiredCapacity](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/SetDesiredCapacity)
  + [TerminateInstanceInAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/TerminateInstanceInAutoScalingGroup)
  + [UpdateAutoScalingGroup](https://docs.aws.amazon.com/goto/SdkForPHPV3/autoscaling-2011-01-01/UpdateAutoScalingGroup)