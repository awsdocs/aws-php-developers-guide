# Creating and managing transcoding jobs in AWS Elemental MediaConvert with AWS SDK for PHP Version 3<a name="emc-examples-jobs"></a>

In this example, you use the AWS SDK for PHP Version 3 to call AWS Elemental MediaConvert and create a transcoding job\. Before you begin, you need to upload the input video to the Amazon S3 bucket you provisioned for the input storage\. For a list of supported input video codecs and containers, see [Supported Input Codecs and Containers](https://docs.aws.amazon.com/mediaconvert/latest/ug/reference-codecs-containers-input.html) in the [AWS Elemental MediaConvert User Guide](https://docs.aws.amazon.com/mediaconvert/latest/ug/)\.

The following examples show how to:
+ Create transcoding jobs in AWS Elemental MediaConvert\. [CreateJob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html#createjob)\.
+ Cancel a transcoding job from the AWS Elemental MediaConvert queue\. [CancelJob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html#canceljob) 
+ Retrieve the JSON for a completed transcoding job\. [GetJob](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html#getjob) 
+ Retrieve a JSON array for up to 20 of the most recently created jobs\. [ListJobs](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html#listjobs) 

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

To access the MediaConvert client, create an IAM role that gives AWS Elemental MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set Up IAM Permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the [AWS Elemental MediaConvert User Guide](https://docs.aws.amazon.com/mediaconvert/latest/ug/)\.

## Create a client<a name="create-a-client"></a>

Configure the AWS SDK for PHP by creating a MediaConvert client, with the region for your code\. In this example, the region is set to us\-west\-2\. Because AWS Elemental MediaConvert uses custom endpoints for each account, configure the `AWS.MediaConvert client class` to use your account\-specific endpoint\. To do this, set the endpoint parameter to your [account\-specific endpoint](emc-examples-getendpoint.md)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\MediaConvert\MediaConvertClient;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$mediaConvertClient = new MediaConvertClient([
    'version' => '2017-08-29',
    'region' => 'us-east-2',
    'profile' => 'default',
    'endpoint' => 'ACCOUNT_ENDPOINT'
]);
```

## Defining a simple transcoding job<a name="defining-a-simple-transcoding-job"></a>

Create the JSON that defines the transcode job parameters\.

These parameters are detailed\. You can use the [AWS Elemental MediaConvert console](https://console.aws.amazon.com/mediaconvert/home) to generate the JSON job parameters by choosing your job settings in the console, and then choosing **Show job JSON** at the bottom of the **Job** section\. This example shows the JSON for a simple job\.

 **Sample Code** 

```
$jobSetting = [
    "OutputGroups" => [
        [
            "Name" => "File Group",
            "OutputGroupSettings" => [
                "Type" => "FILE_GROUP_SETTINGS",
                "FileGroupSettings" => [
                    "Destination" => "s3://OUTPUT_BUCKET_NAME/"
                ]
            ],
            "Outputs" => [
                [
                    "VideoDescription" => [
                        "ScalingBehavior" => "DEFAULT",
                        "TimecodeInsertion" => "DISABLED",
                        "AntiAlias" => "ENABLED",
                        "Sharpness" => 50,
                        "CodecSettings" => [
                            "Codec" => "H_264",
                            "H264Settings" => [
                                "InterlaceMode" => "PROGRESSIVE",
                                "NumberReferenceFrames" => 3,
                                "Syntax" => "DEFAULT",
                                "Softness" => 0,
                                "GopClosedCadence" => 1,
                                "GopSize" => 90,
                                "Slices" => 1,
                                "GopBReference" => "DISABLED",
                                "SlowPal" => "DISABLED",
                                "SpatialAdaptiveQuantization" => "ENABLED",
                                "TemporalAdaptiveQuantization" => "ENABLED",
                                "FlickerAdaptiveQuantization" => "DISABLED",
                                "EntropyEncoding" => "CABAC",
                                "Bitrate" => 5000000,
                                "FramerateControl" => "SPECIFIED",
                                "RateControlMode" => "CBR",
                                "CodecProfile" => "MAIN",
                                "Telecine" => "NONE",
                                "MinIInterval" => 0,
                                "AdaptiveQuantization" => "HIGH",
                                "CodecLevel" => "AUTO",
                                "FieldEncoding" => "PAFF",
                                "SceneChangeDetect" => "ENABLED",
                                "QualityTuningLevel" => "SINGLE_PASS",
                                "FramerateConversionAlgorithm" => "DUPLICATE_DROP",
                                "UnregisteredSeiTimecode" => "DISABLED",
                                "GopSizeUnits" => "FRAMES",
                                "ParControl" => "SPECIFIED",
                                "NumberBFramesBetweenReferenceFrames" => 2,
                                "RepeatPps" => "DISABLED",
                                "FramerateNumerator" => 30,
                                "FramerateDenominator" => 1,
                                "ParNumerator" => 1,
                                "ParDenominator" => 1
                            ]
                        ],
                        "AfdSignaling" => "NONE",
                        "DropFrameTimecode" => "ENABLED",
                        "RespondToAfd" => "NONE",
                        "ColorMetadata" => "INSERT"
                    ],
                    "AudioDescriptions" => [
                        [
                            "AudioTypeControl" => "FOLLOW_INPUT",
                            "CodecSettings" => [
                                "Codec" => "AAC",
                                "AacSettings" => [
                                    "AudioDescriptionBroadcasterMix" => "NORMAL",
                                    "RateControlMode" => "CBR",
                                    "CodecProfile" => "LC",
                                    "CodingMode" => "CODING_MODE_2_0",
                                    "RawFormat" => "NONE",
                                    "SampleRate" => 48000,
                                    "Specification" => "MPEG4",
                                    "Bitrate" => 64000
                                ]
                            ],
                            "LanguageCodeControl" => "FOLLOW_INPUT",
                            "AudioSourceName" => "Audio Selector 1"
                        ]
                    ],
                    "ContainerSettings" => [
                        "Container" => "MP4",
                        "Mp4Settings" => [
                            "CslgAtom" => "INCLUDE",
                            "FreeSpaceBox" => "EXCLUDE",
                            "MoovPlacement" => "PROGRESSIVE_DOWNLOAD"
                        ]
                    ],
                    "NameModifier" => "_1"
                ]
            ]
        ]
    ],
    "AdAvailOffset" => 0,
    "Inputs" => [
        [
            "AudioSelectors" => [
                "Audio Selector 1" => [
                    "Offset" => 0,
                    "DefaultSelection" => "NOT_DEFAULT",
                    "ProgramSelection" => 1,
                    "SelectorType" => "TRACK",
                    "Tracks" => [
                        1
                    ]
                ]
            ],
            "VideoSelector" => [
                "ColorSpace" => "FOLLOW"
            ],
            "FilterEnable" => "AUTO",
            "PsiControl" => "USE_PSI",
            "FilterStrength" => 0,
            "DeblockFilter" => "DISABLED",
            "DenoiseFilter" => "DISABLED",
            "TimecodeSource" => "EMBEDDED",
            "FileInput" => "s3://INPUT_BUCKET_AND_FILE_NAME"
        ]
    ],
    "TimecodeConfig" => [
        "Source" => "EMBEDDED"
    ]
];
```

## Create a job<a name="create-a-job"></a>

After creating the job parameters JSON, call the createJob method by invoking an `AWS.MediaConvert service object`, and passing the parameters\. The ID of the job created is returned in the response data\.

 **Sample Code** 

```
try {
    $result = $mediaConvertClient->createJob([
        "Role" => "IAM_ROLE_ARN",
        "Settings" => $jobSetting, //JobSettings structure
        "Queue" => "JOB_QUEUE_ARN",
        "UserMetadata" => [
            "Customer" => "Amazon"
        ],
    ]);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve a job<a name="retrieve-a-job"></a>

With the JobID returned when you called createjob, you can get detailed descriptions of recent jobs in JSON format\.

 **Sample Code** 

```
$mediaConvertClient = new MediaConvertClient([
    'version' => '2017-08-29',
    'region' => 'us-east-2',
    'profile' => 'default',
    'endpoint' => 'ACCOUNT_ENDPOINT'
]);

try {
    $result = $mediaConvertClient->getJob([
        'Id' => 'JOB_ID',
    ]);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Cancel a job<a name="cancel-a-job"></a>

With the JobID returned when you called createjob, you can cancel a job while it is still in the queue\. You can’t cancel jobs that have already started transcoding\.

 **Sample Code** 

```
$mediaConvertClient = new MediaConvertClient([
    'version' => '2017-08-29',
    'region' => 'us-east-2',
    'profile' => 'default',
    'endpoint' => 'ACCOUNT_ENDPOINT'
]);

try {
    $result = $mediaConvertClient->cancelJob([
        'Id' => 'JOB_ID', // REQUIRED The Job ID of the job to be cancelled.
    ]);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Listing recent transcoding jobs<a name="listing-recent-transcoding-jobs"></a>

Create the parameters JSON, including values to specify whether to sort the list in ASCENDING, or DESCENDING order, the ARN of the job queue to check, and the status of jobs to include\. This returns up to 20 Jobs\. To retrieve the 20 next most recent jobs use the nextToken string returned with result\.

 **Sample Code** 

```
$mediaConvertClient = new MediaConvertClient([
    'version' => '2017-08-29',
    'region' => 'us-east-2',
    'profile' => 'default',
    'endpoint' => 'ACCOUNT_ENDPOINT'
]);

try {
    $result = $mediaConvertClient->listJobs([
        'MaxResults' => 20,
        'Order' => 'ASCENDING',
        'Queue' => 'QUEUE_ARN',
        'Status' => 'SUBMITTED',
        // 'NextToken' => '<string>', //OPTIONAL To retrieve the twenty next most recent jobs
    ]);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```