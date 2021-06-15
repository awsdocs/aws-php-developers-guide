# Set Up SDK Metrics for the AWS SDK for PHP Version 3<a name="guide_sdk-metrics-configure"></a>

The following steps demonstrate how to set up SDK Metrics for the AWS SDK for PHP\. These steps pertain to an Amazon EC2 instance running Amazon Linux for a client application that is using the AWS SDK for PHP\. SDK Metrics is also available for your production environments if you enable it while configuring the AWS SDK for PHP\.

To use SDK Metrics, run the latest version of the CloudWatch agent\. Learn how to [Configure the CloudWatch Agent for SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Configure-CloudWatch-Agent-SDK-Metrics.html) in the Amazon CloudWatch User Guide\.

For more details about IAM Permissions for SDK Metrics, check out the [IAM Permissions for SDK Metrics for AWS SDK for PHP](guide_sdk-metrics-set-permissions.md) article\.

To set up SDK Metrics with the AWS SDK for PHP, follow these instructions:

1. Create an application with an AWS SDK for PHP client to use an AWS service\.

1. Host your project on an Amazon EC2 instance or in your local environment\.

1. Install and use the latest version of the AWS SDK for PHP\.

1. Install and configure a CloudWatch agent on an EC2 instance or in your local environment\.

1. Authorize SDK Metrics to collect and send metrics\.

1.  [Enable SDK Metrics for the AWS SDK for PHP](#csm-enable-agent)\.

For more information, see the following:
+  [Update a CloudWatch Agent](#csm-update-agent) 
+  [Disable SDK Metrics](#csm-disable-agent) 

## Enable SDK Metrics for the AWS SDK for PHP<a name="csm-enable-agent"></a>

By default, SDK Metrics is turned off, host is set to ‘127\.0\.0\.1’ and the port is set to 31000\. The following are the default parameters\.

```
//default values
 [
     'enabled' => false,
     'host' => '127.0.0.1',
     'port' => 31000,
 ]
```

Enabling SDK Metrics is independent of configuring your credentials to use an AWS service\.

You can enable SDK Metrics by passing in a client configuration option, setting environment variables, or by using the AWS Shared config file\.

 **Order of Precedence** 

The order of precedence is as follows \(1 overrides 2\-3, etc\.\):

1. Client configuration option

1. Environment variables

1. AWS Shared config file

### Option 1: Client Configuration Option<a name="option-1-client-configuration-option"></a>

You can pass in a `csm` option to your client constructor to enable and set the configuration options\. This option can be an associative array, an instance of `\Aws\ClientSideMonitoring\ConfigurationInterface`, a callable that provides an instance of `\Aws\ClientSideMonitoring\ConfigurationInterface`, or the boolean value of `false`\.

 **Associative array:** 

```
$client = new \Aws\S3\S3Client([
    'region' => 'your-region',
    'version' => 'latest',
    'csm' => [
        'enabled' => true,
        'host' => 'my.host',
        'port' => 1234,
        'client_id' => 'My Application'
     ]
]);
```

 **Instance of \\Aws\\ClientSideMonitoring\\ConfigurationInterface:** 

```
$client = new \Aws\S3\S3Client([
    'region' => 'your-region',
    'version' => 'latest',
    'csm' => new \Aws\ClientSideMonitoring\Configuration(
        true,
        'my.host',
        1234,
        'My Application'
    )
]);
```

 **Callable:** 

```
$client = new \Aws\S3\S3Client([
    'region' => 'your-region',
    'version' => 'latest',
    'csm' => function() {
        return new \Aws\ClientSideMonitoring\Configuration(
            true,
            '127.0.0.1',
            1234,
            'My Application'
        );
    }
]);
```

 **Boolean `false`:** 

```
$client = new \Aws\S3\S3Client([
    'region' => 'your-region',
    'version' => 'latest',
    'csm' => false
]);
```

### Option 2: Set Environment Variables<a name="option-2-set-environment-variables"></a>

The SDK first checks the profile specified in the environment variable under `AWS_PROFILE` to determine if SDK Metrics is enabled\.

To turn on SDK Metrics, add the following to your environmental variables\.

```
export AWS_CSM_ENABLED=true
```

 [Other configuration settings](#csm-update-agent) are available\. For more information about using shared files, see [Using Credentials from Environment Variables](guide_credentials_environment.md)\.

**Note**  
Enabling SDK Metrics does not configure your credentials to use an AWS service\. To do that, see [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\.

### Option 3: AWS Shared Config File<a name="option-3-aws-shared-config-file"></a>

If no CSM configuration is found in the environment variables, the SDK looks for your default AWS profile field in a shared configuration file\. Typically this is in `~/.aws/config`, but you can change which file is used by setting the `AWS_CONFIG_FILE` environment variable\. If `AWS_DEFAULT_PROFILE` is set to something other than default, update that profile\. To enable SDK Metrics, add `csm_enabled` to the shared config file\.

```
[default]
csm_enabled = true

[profile aws_csm]
csm_enabled = true
```

 [Other configuration settings](#csm-update-agent) are available\. For more information about using AWS Shared files, see [Using the AWS Credentials File and Credential Profiles](guide_credentials_profiles.md)\.

**Note**  
Enabling SDK Metrics does not configure your credentials to use an AWS service\. To do that, see [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\.

## Update a CloudWatch Agent<a name="csm-update-agent"></a>

To make changes to the host or port ID, you need to set the values and then restart any AWS jobs that are currently active\.

### Option 1: Client Configuration Option<a name="id1"></a>

See Option 1 above under “Enable SDK Metrics for the AWS SDK for PHP” for examples of how to set the client configuration `csm` option to modify CSM settings\.

### Option 2: Set Environment Variables<a name="id2"></a>

Most AWS services use the default port\. But if the service you want SDK Metrics to monitor uses a unique port, add *AWS\_CSM\_PORT=\[port\_number\]*, to the host’s environment variables\. Additionally, a different host can be specified using the *AWS\_CSM\_HOST* environment variable\.

```
export AWS_CSM_ENABLED=true
export AWS_CSM_PORT=1234
export AWS_CSM_HOST=192.168.0.1
```

### Option 3: AWS Shared Config File<a name="id3"></a>

Most services use the default port\. But if your service requires a unique port ID, add *csm\_port = \[port\_number\]* to *\~/\.aws/config* \(or the file specified by the environment variable `AWS_CONFIG_FILE`\)\. A non\-default host can be configured using *csm\_host*\.

```
[default]
csm_enabled = false
csm_host = 123.4.5.6
csm_port = 1234

[profile aws_csm]
csm_enabled = false
csm_host = 123.4.5.6
csm_port = 1234
```

### Restart SDK Metrics<a name="restart-sdkm"></a>

To restart a job, run the following commands\.

```
amazon-cloudwatch-agent-ctl –a stop;
amazon-cloudwatch-agent-ctl –a start;
```

## Disable SDK Metrics<a name="csm-disable-agent"></a>

To turn off SDK Metrics, set *csm\_enabled* to *false* in your environment variables, or in your AWS Shared config file located at `~/.aws/config` \(or the file specified by the environment variable `AWS_CONFIG_FILE`\)\. Then restart your CloudWatch agent so that the changes can take effect\.

### Set csm\_enabled to false<a name="set-csm-enabled-to-false"></a>

**Note**  
Note the order of precedence listed above\. For example, if SDK Metrics is enabled in the environment variables but disabled in the config file, the SDK Metrics remains enabled\.

 **Option 1: Client configuration** 

```
$client = new \Aws\S3\S3Client([
    'region' => 'your-region',
    'version' => 'latest',
    'csm' => false
]);
```

 **Option 2: Environment Variables** 

```
export AWS_CSM_ENABLED=false
```

 **Option 3: AWS Shared Config File** 

```
[default]
csm_enabled = false

[profile aws_csm]
csm_enabled = false
```

### Stop SDK Metrics and Restart CloudWatch agent<a name="stop-sdkm-and-restart-cw-agent"></a>

To disable SDK Metrics, use the following command\.

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a stop &&
echo "Done"
```

If you are using other CloudWatch features, restart CloudWatch with the following command\.

```
amazon-cloudwatch-agent-ctl –a start;
```