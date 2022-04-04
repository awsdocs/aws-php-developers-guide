# Managing Amazon CloudFront Invalidations Using the CloudFront API and the AWS SDK for PHP Version 3<a name="cloudfront-example-invalidation"></a>

Amazon CloudFront caches copies of static and dynamic files in worldwide edge locations\. To remove or update a file on all edge locations, create an invalidation for each file or for a group of files\.

Each calendar month, your first 1,000 invalidations are free\. To learn more about removing content from a CloudFront edge location, see [Invalidating Files](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html)\.

The following examples show how to:
+ Create a distribution invalidation using [CreateInvalidation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#createinvalidation)\.
+ Get a distribution invalidation using [GetInvalidation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#getinvalidation)\.
+ List distributions using [ListInvalidations](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-cloudfront-2018-11-05.html#listinvalidations)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

For more information about using Amazon CloudFront, see the [Amazon CloudFront Developer Guide](Amazon CloudFront Developer Guide)\.

## Create a Distribution Invalidation<a name="create-a-distribution-invalidation"></a>

Create a CloudFront distribution invalidation by specifying the path location for the files you need to remove\. This example invalidates all files in the distribution, but you can identify specific files under `Items`\.

To create a CloudFront distribution invalidation, use the [CreateInvalidation](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateInvalidation.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function createInvalidation($cloudFrontClient, $distributionId, 
    $callerReference, $paths, $quantity)
{
    try {
        $result = $cloudFrontClient->createInvalidation([
            'DistributionId' => $distributionId,
            'InvalidationBatch' => [
                'CallerReference' => $callerReference,
                'Paths' => [
                    'Items' => $paths,
                    'Quantity' => $quantity,
                ],
            ]
        ]);

        $message = '';

        if (isset($result['Location']))
        {
            $message = 'The invalidation location is: ' . 
                $result['Location'];
        }

        $message .= ' and the effective URI is ' . 
            $result['@metadata']['effectiveUri'] . '.';

        return $message;
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function createTheInvalidation()
{
    $distributionId = 'E1WICG14DUW2AP'; // 'E17G7YNEXAMPLE';
    $callerReference = 'my-unique-value';
    $paths = ['/*'];
    $quantity = 1;

    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);

    echo createInvalidation($cloudFrontClient, $distributionId, 
        $callerReference, $paths, $quantity);
}

// Uncomment the following line to run this code in an AWS account.
// createTheInvalidation();
```

## Get a Distribution Invalidation<a name="get-a-distribution-invalidation"></a>

To retrieve the status and details about a CloudFront distribution invalidation, use the [GetInvalidation](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetInvalidation.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function getInvalidation($cloudFrontClient, $distributionId, $invalidationId)
{
    try {
        $result = $cloudFrontClient->getInvalidation([
            'DistributionId' => $distributionId,
            'Id' => $invalidationId,
        ]);

        $message = '';

        if (isset($result['Invalidation']['Status']))
        {
            $message = 'The status for the invalidation with the ID of ' . 
                $result['Invalidation']['Id'] . ' is ' . 
                $result['Invalidation']['Status'];
        } 
        
        if (isset($result['@metadata']['effectiveUri']))
        {
            $message .= ', and the effective URI is ' . 
                $result['@metadata']['effectiveUri'] . '.';
        } else {
            $message = 'Error: Could not get information about ' .
                'the invalidation. The invalidation\'s status ' .
                'was not available.';
        }
        
        return $message;

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function getsAnInvalidation()
{
    $distributionId = 'E1BTGP2EXAMPLE';
    $invalidationId = 'I1CDEZZEXAMPLE';

    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);
    
    echo getInvalidation($cloudFrontClient, $distributionId, $invalidationId);
}

// Uncomment the following line to run this code in an AWS account.
// getsAnInvalidation();
```

## List Distribution Invalidations<a name="list-distribution-invalidations"></a>

To list all current CloudFront distribution invalidations, use the [ListInvalidations](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListInvalidations.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudFront\CloudFrontClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function listInvalidations($cloudFrontClient, $distributionId)
{
    try {
        $result = $cloudFrontClient->listInvalidations([
            'DistributionId' => $distributionId
        ]);
        return $result;
    } catch (AwsException $e) {
        exit('Error: ' . $e->getAwsErrorMessage());
    }
}

function listTheInvalidations()
{
    $distributionId = 'E1WICG1EXAMPLE';

    $cloudFrontClient = new Aws\CloudFront\CloudFrontClient([
        'profile' => 'default',
        'version' => '2018-06-18',
        'region' => 'us-east-1'
    ]);

    $invalidations = listInvalidations($cloudFrontClient, 
        $distributionId);

    if (isset($invalidations['InvalidationList']))
    {
        if ($invalidations['InvalidationList']['Quantity'] > 0)
        {
            foreach ($invalidations['InvalidationList']['Items'] as $invalidation)
            {
                echo 'The invalidation with the ID of ' . $invalidation['Id'] . 
                    ' has the status of ' . $invalidation['Status'] . '.' . "\n";
            }
        } else {
            echo 'Could not find any invalidations for the specified distribution.';
        }
    } else {
        echo 'Error: Could not get invalidation information. Could not get ' . 
            'information about the specified distribution.';
    }   
}

// Uncomment the following line to run this code in an AWS account.
// listTheInvalidations();
```