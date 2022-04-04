# Signing Custom Amazon CloudSearch Domain Requests with AWS SDK for PHP Version 3<a name="service_cloudsearch-custom-requests"></a>

Amazon CloudSearch domain requests can be customized beyond what is supported by the AWS SDK for PHP\. In cases where you need to make custom requests to domains protected by IAM authentication, you can use the SDK’s credential providers and signers to sign any [PSR\-7 request](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Psr.Http.Message.RequestInterface.html)\.

For example, if you’re following [Cloud Search’s Getting Started guide](https://docs.aws.amazon.com/cloudsearch/latest/developerguide/getting-started.html) and want to use an IAM\-protected domain for [Step 3](https://docs.aws.amazon.com/cloudsearch/latest/developerguide/getting-started-search.html), you would need to sign and execute your request as follows\.

The following examples show how to:
+ Sign a request with the AWS signing protocol using [SignatureV4](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Signature.SignatureV4.html#_signRequest)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Sign CSlong Domain Request<a name="sign-cslong-domain-request"></a>

 **Imports** 

```
require './vendor/autoload.php';

use Aws\Credentials\CredentialProvider;
use Aws\Signature\SignatureV4;
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Request;
```

 **Sample Code** 

```
function searchDomain($client, $domainName, $domainId,
    $domainRegion, $searchString)
{
    $domainPrefix = 'search-';
    $cloudSearchDomain = 'cloudsearch.amazonaws.com';
    $cloudSearchVersion = '2013-01-01';
    $searchPrefix = 'search?';

    // Specify the search to send.
    $request = new Request(
        'GET',
        "https://{$domainPrefix}{$domainName}-{$domainId}.{$domainRegion}." .
            "{$cloudSearchDomain}/{$cloudSearchVersion}/" .
            "{$searchPrefix}{$searchString}"
    );

    // Get default AWS account access credentials.
    $credentials = call_user_func(CredentialProvider::defaultProvider())->wait();

    // Sign the search request with the credentials.
    $signer = new SignatureV4('cloudsearch', $domainRegion);
    $request = $signer->signRequest($request, $credentials);

    // Send the signed search request.
    $response = $client->send($request);

    // Report the search results, if any.
    $results = json_decode($response->getBody());

    $message = '';

    if ($results->hits->found > 0) {

        $message .= 'Search results:' . "\n";

        foreach($results->hits->hit as $hit)
        {
            $message .= $hit->fields->title . "\n";
        }
    } else {
        $message .= 'No search results.';
    }

    return $message;
}

function searchADomain()
{
    $domainName = 'my-search-domain';
    $domainId = '7kbitd6nyiglhdtmssxEXAMPLE';
    $domainRegion = 'us-east-1';
    $searchString = 'q=star+wars&return=title';
    $client = new Client();

    echo searchDomain($client, $domainName, $domainId, 
        $domainRegion, $searchString);
}

// Uncomment the following line to run this code in an AWS account.
// searchADomain();
```