.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


################################################
|CFlong| Examples Using the |sdk-php| Version 3
################################################

.. meta::
   :description: Amazon CloudFront code examples for the AWS SDK for PHP version 3.
   :keywords: Amazon CloudFront code examples for PHP, Amazon CloudFront Data for PHP.

|CFlong| is an AWS web service that speeds up serving static and dynamic web content from your own web 
server or an AWS server, such as |S3|. |CF| delivers content through a worldwide network of data 
centers called edge locations. When a user requests content that you're distributing with |CF|, they’re 
routed to the edge location that provides the lowest latency. If the content isn’t cached there already, 
|CF| retrieves a copy from the origin server, serves it, and then caches it for future requests. 

For more information about |CF|, see the |CF-dg|_.

.. image:: images/code-samples-cloudfront.png
   :alt: An overview of how AWS SDK for PHP connects to Amazon CloudFront

All the example code for the |sdk-php| Version 3 is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

.. toctree::
   :maxdepth: 1

   Managing CloudFront Distributions <cloudfront-example-distribution>
   Managing CloudFront Invalidations <cloudfront-example-invalidation>
   Signing CloudFront URLs <cloudfront-example-signed-url>
