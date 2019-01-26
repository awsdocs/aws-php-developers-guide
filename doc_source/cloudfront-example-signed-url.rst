.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################################
Signing |CFlong| URLs with |sdk-php| Version 3
##############################################

.. meta::
   :description: Programing CloudFront using the AWS SDK for PHP version 3.
   :keywords: CloudFront, AWS SDK for PHP version 3 examples, CloudFront for PHP code examples
   
Signed URLs enable you to provide users access to your private content. A signed
URL includes additional information (for example, expiration time) that gives you more
control over access to your content. This additional information appears in a
policy statement, which is based on either a canned policy or a custom policy.
For information about how to set up private distributions and why you need to
sign URLs, see :CF-dg:`Serving Private Content through Amazon CloudFront
<PrivateContent>` in the |cf-dg|.

.. note:

    You must have the OpenSSL extension installed in your PHP environment
    to sign |CF| URLs.

* Create a signed Amazon CloudFront URL using :aws-php-class:`getSignedURL <class-Aws.CloudFront.CloudFrontClient.html#_getSignedUrl>`.
* Create a signed Amazon CloudFront cookie using :aws-php-class:`getSignedCookie <class-Aws.CloudFront.CloudFrontClient.html#_getSignedCookie>`.

.. include:: text/git-php-examples.txt

For more information about using |CFlong|, see the |CF-dg|_.
   
Signing |CF| URLs for Private Distributions
===========================================

You can sign a URL using the |CF| client in the SDK. First, you must
create a ``CloudFrontClient`` object.
You can sign a |CF| URL for a video resource using either a canned or
custom policy.

**Imports**

.. literalinclude::  cloudfront.php.private_distribution.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.private_distribution.main.txt
   :language: PHP

Use a Custom Policy When Creating |CF| URLs
===========================================

To use a custom policy, provide the ``policy`` key instead of ``expires``.

**Imports**

.. literalinclude::  cloudfront.php.private_distribution_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.private_distribution_policy.main.txt
   :language: PHP

Use a |CF| Signed URL
==========================

The form of the signed URL differs, depending on whether the URL you
are signing is using the "HTTP" or "RTMP" scheme. In the case of "HTTP", the
full, absolute URL is returned. For "RTMP", only the relative URL is returned
for your convenience. This is because some players require the host and path to be
provided as separate parameters.

The following example shows how you could use the signed URL to construct a
webpage that displays a video using `JWPlayer <http://www.longtailvideo.com/jw-player/>`_.
The same type of technique would apply to other players such as `FlowPlayer <http://flowplayer.org/>`_,
but require different client-side code.

.. code-block:: html

    <html>
    <head>
        <title>|CFlong| Streaming Example</title>
        <script type="text/javascript" src="https://example.com/jwplayer.js"></script>
    </head>
    <body>
        <div id="video">The canned policy video will be here.</div>
        <script type="text/javascript">
            jwplayer('video').setup({
                file: "<?= $streamHostUrl ?>/cfx/st/<?= $signedUrlCannedPolicy ?>",
                width: "720",
                height: "480"
            });
        </script>
    </body>
    </html>

Signing |CF| Cookies for Private Distributions
==============================================

As an alternative to signed URLs, you can also grant clients access to a private
distribution via signed cookies. Signed cookies enable you to provide access to
multiple restricted files, such as all of the files for a video in HLS format or
all of the files in the subscribers' area of a website. For more information on
why you might want to use signed cookies instead of signed URLs (or vice versa),
see :CF-dg:`Choosing Between Signed URLs and Signed Cookies <private-content-choosing-signed-urls-cookies>` in the |cf-dg|.

.. note:

    Signed cookies are not supported for RTMP distributions. Use signed URLs
    instead.

Creating a signed cookie is similar to creating a signed URL. The only
difference is the method that is called (``getSignedCookie`` instead of ``getSignedUrl``).

**Imports**

.. literalinclude::  cloudfront.php.signed_cookie.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.signed_cookie.main.txt
   :language: PHP

Use a Custom Policy When Creating |CF| Cookies 
===============================================

As with ``getSignedUrl``, you can provide a ``'policy'`` parameter instead of an
``expires`` parameter and a ``url`` parameter to sign a cookie with a custom
policy. A custom policy can contain wildcards in the resource key. This enables you
to create a single signed cookie for multiple files.

``getSignedCookie`` returns an array of key-value pairs, all of which must
be set as cookies to grant access to a private distribution.

**Imports**

.. literalinclude::  cloudfront.php.signed_cookie_policy.import.txt
   :language: PHP

**Sample Code**

.. literalinclude:: cloudfront.php.signed_cookie_policy.main.txt
   :language: PHP


Send |CF| Cookies to Guzzle Client
==================================

You can also pass these cookies to a ``GuzzleHttp\Cookie\CookieJar`` for use
with a Guzzle client.

.. code-block:: php

    use GuzzleHttp\Client;
    use GuzzleHttp\Cookie\CookieJar;

    $distribution = "example-distribution.cloudfront.net";
    $client = new \GuzzleHttp\Client([
        'base_uri' => "https://$distribution",
        'cookies' => CookieJar::fromArray($signedCookieCustomPolicy, $distribution),
    ]);

    $client->get('video.mp4');

For more information, see :CF-dg:`Using Signed Cookies <private-content-signed-cookies>`
in the |cf-dg|.
