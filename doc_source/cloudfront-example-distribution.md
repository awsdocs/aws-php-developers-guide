# Managing Amazon CloudFront Distributions Using the CloudFront API and the AWS SDK for PHP Version 3<a name="cloudfront-example-distribution"></a>

Amazon CloudFront caches content in worldwide edge locations to speed up distribution of static and dynamic files that you store on your own server, or on an Amazon service like Amazon S3 and Amazon EC2\. When users request content from your website, CloudFront serves it from the closest edge location, if the file is cached there\. Otherwise CloudFront retrieves a copy of the file, serves it, and then caches it for the next request\. Caching content at an edge location reduces the latency of similar user requests in that area\.

For each CloudFront distribution that you create, you specify where the content is located and how to distribute it when users make requests\. This topic focuses on distributions for static and dynamic files such as HTML, CSS, JSON, and image files\. F or information about using CloudFront with video on demand, see [On\-Demand and Live Streaming Video with CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/on-demand-streaming-video.html)\.

The following examples show how to:
+ Create a distribution using [CreateDistribution](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#createdistribution)\.
+ Get a distribution using [GetDistribution](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#getdistribution)\.
+ List distributions using [ListDistributions](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#listdistributions)\.
+ Update distributions using [UpdateDistributions](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#updatedistribution)\.
+ Disable distributions using [DisableDistribution](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#disabledistribution)\.
+ Delete distributions using [DeleteDistributions](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#deletedistribution)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

For more information about using Amazon CloudFront, see the [Amazon CloudFront Developer Guide](Amazon CloudFront Developer Guide)\.

## Create a CloudFront Distribution<a name="create-a-cf-distribution"></a>

Create a distribution from an Amazon S3 bucket\. In the following example, optional parameters are commented out, but default values are displayed\. To add customizations to your distribution, uncomment both the value and the parameter inside `$distribution`\.

To create a CloudFront distribution, use the [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function createS3Distribution($cloudFrontClient, $distribution)
{
    try {
        $result = $cloudFrontClient->createDistribution([
            'DistributionConfig' => $distribution
        ]);

        $message = '';

        if (isset($result['Distribution']['Id']))
        {
            $message = 'Distribution created with the ID of ' .
                $result['Distribution']['Id'];
        }
        
        $message .= ' and an effective URI of ' . 
            $result['@metadata']['effectiveUri'] . '.';

        return $message;
    } catch (AwsException $e) {
        return 'Error: ' . $e['message'];
    }
}

function createsTheS3Distribution()
{
    $originName = 'my-unique-origin-name';
    $s3BucketURL = 'my-bucket-name.s3.amazonaws.com';
    $callerReference = 'my-unique-caller-reference';
    $comment = 'my-comment-about-this-distribution';
    $defaultCacheBehavior = [
        'AllowedMethods' => [
            'CachedMethods' => [
                'Items' => ['HEAD', 'GET'],
                'Quantity' => 2
            ],
            'Items' => ['HEAD', 'GET'],
            'Quantity' => 2
        ],
        'Compress' => false,
        'DefaultTTL' => 0,
        'FieldLevelEncryptionId' => '',
        'ForwardedValues' => [
            'Cookies' => [
                'Forward' => 'none'
            ],
            'Headers' => [
                'Quantity' => 0
            ],
            'QueryString' => false,
            'QueryStringCacheKeys' => [
                'Quantity' => 0
            ]
        ],
        'LambdaFunctionAssociations' => ['Quantity' => 0],
        'MaxTTL' => 0,
        'MinTTL' => 0,
        'SmoothStreaming' => false,
        'TargetOriginId' => $originName,
        'TrustedSigners' => [
            'Enabled' => false,
            'Quantity' => 0
        ],
        'ViewerProtocolPolicy' => 'allow-all'
    ];
    $enabled = false;
    $origin = [
        'Items' => [
            [
                'DomainName' => $s3BucketURL,
                'Id' => $originName,
                'OriginPath' => '',
                'CustomHeaders' => ['Quantity' => 0],
                'S3OriginConfig' => ['OriginAccessIdentity' => '']
            ]
        ],
        'Quantity' => 1
    ];
    $distribution = [
        'CallerReference' => $callerReference,
        'Comment' => $comment,
        'DefaultCacheBehavior' => $defaultCacheBehavior,
        'Enabled' => $enabled,
        'Origins' => $origin
    ];
    
    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);
    
    echo createS3Distribution($cloudFrontClient, $distribution);
}

// Uncomment the following line to run this code in an AWS account.
// createsTheS3Distribution();
```

## Retrieve a CloudFront Distribution<a name="retrieve-a-cf-distribution"></a>

To retrieve the status and details of a specified CloudFront distribution, use the [GetDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetDistribution.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function getDistribution($cloudFrontClient, $distributionId)
{
    try {
        $result = $cloudFrontClient->getDistribution([
            'Id' => $distributionId
        ]);

        $message = '';

        if (isset($result['Distribution']['Status']))
        {
            $message = 'The status of the distribution with the ID of ' . 
                $result['Distribution']['Id'] . ' is currently ' . 
                $result['Distribution']['Status'];
        }
        
        if (isset($result['@metadata']['effectiveUri']))
        {
            $message .= ', and the effective URI is ' . 
                $result['@metadata']['effectiveUri'] . '.';
        } else {
            $message = 'Error: Could not get the specified distribution. ' .
                'The distribution\'s status is not available.';
        }

        return $message;

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function getsADistribution()
{
    $distributionId = 'E1BTGP2EXAMPLE';

    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);
    
    echo getDistribution($cloudFrontClient, $distributionId);
}

// Uncomment the following line to run this code in an AWS account.
// getsADistribution();
```

## List CloudFront Distributions<a name="list-cf-distributions"></a>

Get a list of existing CloudFront distributions in the specified AWS Region from your current account using the [ListDistributions](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListDistributions.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function listDistributions($cloudFrontClient)
{
    try {
        $result = $cloudFrontClient->listDistributions([]);
        return $result;
    } catch (AwsException $e) {
        exit('Error: ' . $e->getAwsErrorMessage());
    }
}

function listTheDistributions()
{
    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-2'
    ]);

    $distributions = listDistributions($cloudFrontClient);

    if (count($distributions) == 0)
    {
        echo 'Could not find any distributions.';
    } else {
        foreach ($distributions['DistributionList']['Items'] as $distribution)
        {
            echo 'The distribution with the ID of ' . $distribution['Id'] . 
                ' has the status of ' . $distribution['Status'] . '.' . "\n";
        }
    }
}

// Uncomment the following line to run this code in an AWS account.
// listTheDistributions();
```

## Update a CloudFront Distribution<a name="update-a-cf-distribution"></a>

Updating a CloudFront distribution is similar to creating a distribution\. However, when you update a distribution, more fields are required and all values must be included\. To make changes to an existing distribution, we recommend that you first retrieve the existing distribution, and update the values you want to change in the `$distribution` array\.

To update a specified CloudFront distribution, use the [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function updateDistribution($cloudFrontClient, $distributionId, 
    $distributionConfig, $eTag)
{
    try {
        $result = $cloudFrontClient->updateDistribution([
            'DistributionConfig' => $distributionConfig,
            'Id' => $distributionId,
            'IfMatch' => $eTag
        ]);

        return 'The distribution with the following effective URI has ' .
            'been updated: ' . $result['@metadata']['effectiveUri'];

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function getDistributionConfig($cloudFrontClient, $distributionId)
{
    try {
        $result = $cloudFrontClient->getDistribution([
            'Id' => $distributionId,
        ]);

        if (isset($result['Distribution']['DistributionConfig']))
        {
            return [
                'DistributionConfig' => $result['Distribution']['DistributionConfig'],
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        } else {
            return [
                'Error' => 'Error: Cannot find distribution configuration details.',
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        }

    } catch (AwsException $e) {
        return [
            'Error' => 'Error: ' . $e->getAwsErrorMessage()
        ];
    }
}

function getDistributionETag($cloudFrontClient, $distributionId)
{

    try {
        $result = $cloudFrontClient->getDistribution([
            'Id' => $distributionId,
        ]);
        
        if (isset($result['ETag']))
        {
            return [
                'ETag' => $result['ETag'],
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ]; 
        } else {
            return [
                'Error' => 'Error: Cannot find distribution ETag header value.',
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        }

    } catch (AwsException $e) {
        return [
            'Error' => 'Error: ' . $e->getAwsErrorMessage()
        ];
    }
}

function updateADistribution()
{
    // $distributionId = 'E1BTGP2EXAMPLE';
    $distributionId = 'E1X3BKQ569KEMH';

    $cloudFrontClient = new CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);

    // To change a distribution, you must first get the distribution's 
    // ETag header value.
    $eTag = getDistributionETag($cloudFrontClient, $distributionId);

    if (array_key_exists('Error', $eTag)) {
        exit($eTag['Error']);
    }

    // To change a distribution, you must also first get information about 
    // the distribution's current configuration. Then you must use that 
    // information to build a new configuration.
    $currentConfig = getDistributionConfig($cloudFrontClient, $distributionId);

    if (array_key_exists('Error', $currentConfig)) {
        exit($currentConfig['Error']);
    }

    // To change a distribution's configuration, you can set the 
    // distribution's related configuration value as part of a change request, 
    // for example:
    // 'Enabled' => true
    // Some configuration values are required to be specified as part of a change 
    // request, even if you don't plan to change their values. For ones you 
    // don't want to change but are required to be specified, you can just reuse 
    // their current values, as follows. 
    $distributionConfig = [
        'CallerReference' => $currentConfig['DistributionConfig']["CallerReference"], 
        'Comment' => $currentConfig['DistributionConfig']["Comment"], 
        'DefaultCacheBehavior' => $currentConfig['DistributionConfig']["DefaultCacheBehavior"], 
        'DefaultRootObject' => $currentConfig['DistributionConfig']["DefaultRootObject"],
        'Enabled' => $currentConfig['DistributionConfig']["Enabled"], 
        'Origins' => $currentConfig['DistributionConfig']["Origins"], 
        'Aliases' => $currentConfig['DistributionConfig']["Aliases"],
        'CustomErrorResponses' => $currentConfig['DistributionConfig']["CustomErrorResponses"],
        'HttpVersion' => $currentConfig['DistributionConfig']["HttpVersion"],
        'CacheBehaviors' => $currentConfig['DistributionConfig']["CacheBehaviors"],
        'Logging' => $currentConfig['DistributionConfig']["Logging"],
        'PriceClass' => $currentConfig['DistributionConfig']["PriceClass"],
        'Restrictions' => $currentConfig['DistributionConfig']["Restrictions"],
        'ViewerCertificate' => $currentConfig['DistributionConfig']["ViewerCertificate"],
        'WebACLId' => $currentConfig['DistributionConfig']["WebACLId"]
    ];

    echo updateDistribution($cloudFrontClient, $distributionId,
        $distributionConfig, $eTag['ETag']);
}

// Uncomment the following line to run this code in an AWS account.
// updateADistribution();
```

## Disable a CloudFront Distribution<a name="disable-a-cf-distribution"></a>

To deactivate or remove a distribution, change its status from deployed to disabled\.

To disable the specified CloudFront distribution, use the [DisableDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DisableDistribution.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function disableDistribution($cloudFrontClient, $distributionId, 
    $distributionConfig, $eTag)
{
    try {
        $result = $cloudFrontClient->updateDistribution([
            'DistributionConfig' => $distributionConfig,
            'Id' => $distributionId,
            'IfMatch' => $eTag
        ]);
        return 'The distribution with the following effective URI has ' .
            'been disabled: ' . $result['@metadata']['effectiveUri'];
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function getDistributionConfig($cloudFrontClient, $distributionId)
{
    try {
        $result = $cloudFrontClient->getDistribution([
            'Id' => $distributionId,
        ]);

        if (isset($result['Distribution']['DistributionConfig']))
        {
            return [
                'DistributionConfig' => $result['Distribution']['DistributionConfig'],
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        } else {
            return [
                'Error' => 'Error: Cannot find distribution configuration details.',
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        }

    } catch (AwsException $e) {
        return [
            'Error' => 'Error: ' . $e->getAwsErrorMessage()
        ];
    }
}

function getDistributionETag($cloudFrontClient, $distributionId)
{

    try {
        $result = $cloudFrontClient->getDistribution([
            'Id' => $distributionId,
        ]);
        
        if (isset($result['ETag']))
        {
            return [
                'ETag' => $result['ETag'],
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ]; 
        } else {
            return [
                'Error' => 'Error: Cannot find distribution ETag header value.',
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        }

    } catch (AwsException $e) {
        return [
            'Error' => 'Error: ' . $e->getAwsErrorMessage()
        ];
    }
}

function disableADistribution()
{
    $distributionId = 'E1BTGP2EXAMPLE';

    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);

    // To disable a distribution, you must first get the distribution's 
    // ETag header value.
    $eTag = getDistributionETag($cloudFrontClient, $distributionId);

    if (array_key_exists('Error', $eTag)) {
        exit($eTag['Error']);
    }

    // To delete a distribution, you must also first get information about 
    // the distribution's current configuration. Then you must use that 
    // information to build a new configuration, including setting the new
    // configuration to disabled.
    $currentConfig = getDistributionConfig($cloudFrontClient, $distributionId);

    if (array_key_exists('Error', $currentConfig)) {
        exit($currentConfig['Error']);
    }

    $distributionConfig = [
        'CacheBehaviors' => $currentConfig['DistributionConfig']["CacheBehaviors"],
        'CallerReference' => $currentConfig['DistributionConfig']["CallerReference"],
        'Comment' => $currentConfig['DistributionConfig']["Comment"],
        'DefaultCacheBehavior' => $currentConfig['DistributionConfig']["DefaultCacheBehavior"],
        'DefaultRootObject' => $currentConfig['DistributionConfig']["DefaultRootObject"],
        'Enabled' => false,
        'Origins' => $currentConfig['DistributionConfig']["Origins"],
        'Aliases' => $currentConfig['DistributionConfig']["Aliases"],
        'CustomErrorResponses' => $currentConfig['DistributionConfig']["CustomErrorResponses"],
        'HttpVersion' => $currentConfig['DistributionConfig']["HttpVersion"],
        'Logging' => $currentConfig['DistributionConfig']["Logging"],
        'PriceClass' => $currentConfig['DistributionConfig']["PriceClass"],
        'Restrictions' => $currentConfig['DistributionConfig']["Restrictions"],
        'ViewerCertificate' => $currentConfig['DistributionConfig']["ViewerCertificate"],
        'WebACLId' => $currentConfig['DistributionConfig']["WebACLId"]
    ];

    echo disableDistribution($cloudFrontClient, $distributionId,
        $distributionConfig, $eTag['ETag']);
}

// Uncomment the following line to run this code in an AWS account.
// disableADistribution();
```

## Delete a CloudFront Distribution<a name="delete-a-cf-distribution"></a>

Once a distribution is in a disabled status, you can delete the distribution\.

To remove a specified CloudFront distribution, use the [DeleteDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteDistribution.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function deleteDistribution($cloudFrontClient, $distributionId, $eTag)
{
    try {
        $result = $cloudFrontClient->deleteDistribution([
            'Id' => $distributionId,
            'IfMatch' => $eTag
        ]);
        return 'The distribution at the following effective URI has ' . 
            'been deleted: ' . $result['@metadata']['effectiveUri'];
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function getDistributionETag($cloudFrontClient, $distributionId)
{
    try {
        $result = $cloudFrontClient->getDistribution([
            'Id' => $distributionId,
        ]);
        
        if (isset($result['ETag']))
        {
            return [
                'ETag' => $result['ETag'],
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ]; 
        } else {
            return [
                'Error' => 'Error: Cannot find distribution ETag header value.',
                'effectiveUri' => $result['@metadata']['effectiveUri']
            ];
        }

    } catch (AwsException $e) {
        return [
            'Error' => 'Error: ' . $e->getAwsErrorMessage()
        ];
    }
}

function deleteADistribution()
{
    $distributionId = 'E17G7YNEXAMPLE';

    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);

    // To delete a distribution, you must first get the distribution's 
    // ETag header value.
    $eTag = getDistributionETag($cloudFrontClient, $distributionId);

    if (array_key_exists('Error', $eTag)) {
        exit($eTag['Error']);
    } else {
        echo deleteDistribution($cloudFrontClient, $distributionId, 
            $eTag['ETag']);
    }
}

// Uncomment the following line to run this code in an AWS account.
// deleteADistribution();
```