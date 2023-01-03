# Amazon SNS examples using the AWS SDK for PHP Version 3<a name="sns-examples"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\.

In Amazon SNS, there are two types of clients: publishers \(also referred to as producers\) and subscribers \(also referred to as consumers\)\. Publishers communicate asynchronously with subscribers by producing and sending a message to a topic, which is a logical access point and communication channel\. Subscribers \(web servers, email addresses, Amazon SQS queues, AWS Lambda functions\) consume or receive the message or notification over one of the supported protocols \(Amazon SQS, HTTP/HTTPS URLs, email, AWS SMS, Lambda\) when they are subscribed to the topic\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

**Topics**
+ [Managing topics](sns-examples-managing-topics.md)
+ [Managing subscriptions](sns-examples-subscribing-unsubscribing-topics.md)
+ [Sending amazon SMS messages](sns-examples-sending-sms.md)