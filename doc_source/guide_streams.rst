.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##################################
Streams in the |sdk-php| Version 3
##################################

.. meta::
   :description: Creating a Guzzle stream decorator with the AWS SDK for PHP version 3.
   :keywords: AWS SDK for PHP version 3, steam decorators, guzzle, PHP for AWS version 3

As part of its integration of the `PSR-7 <http://www.php-fig.org/psr/psr-7/>`_
HTTP message standard, the |sdk-php| uses the :aws-php-class:`PSR-7 StreamInterface
</class-Psr.Http.Message.StreamInterface.html>`
internally as its abstraction over `PHP streams
<http://php.net/manual/en/intro.stream.php>`_. Any command with an input field
defined as a blob, such as the ``Body`` parameter on an :aws-php-class:`S3::PutObject command
</api-s3-2006-03-01.html#putobject>`,
can be satisfied with a string, a PHP stream resource, or an instance of
``Psr\Http\Message\StreamInterface``.

.. warning::

    The SDK will take ownership of any raw PHP stream resource supplied as an
    input parameter to a command. The stream will be consumed and closed on your
    behalf.

    If you need to share a stream between an SDK operation and your code, wrap
    it in an instance of ``GuzzleHttp\Psr7\Stream`` before including it as a
    command parameter. The SDK will consume the stream, so your code will need
    to account for movement of the stream's internal cursor. Guzzle streams will
    call ``fclose`` on the underlying stream resource when they are destroyed by
    PHP's garbage collector, so you will not need to close the stream yourself.

Stream Decorators
=================

Guzzle provides several stream decorators that you can use to control how the
SDK and Guzzle interact with the streaming resource provided as an input
parameter to a command. These decorators can modify how handlers are able
to read and seek on a given stream. The following is a partial list; more can be
found on the `GuzzleHttp\Psr7 repository <https://github.com/guzzle/psr7>`_.

AppendStream
------------

:aws-php-class:`GuzzleHttp\\Psr7\\AppendStream </class-GuzzleHttp.Psr7.AppendStream.html>`

Reads from multiple streams, one after the other.

.. code-block:: php

    use GuzzleHttp\Psr7;

    $a = Psr7\stream_for('abc, ');
    $b = Psr7\stream_for('123.');
    $composed = new Psr7\AppendStream([$a, $b]);

    $composed->addStream(Psr7\stream_for(' Above all listen to me'));

    echo $composed(); // abc, 123. Above all listen to me.

CachingStream
-------------

:aws-php-class:`GuzzleHttp\\Psr7\\CachingStream </class-GuzzleHttp.Psr7.CachingStream.html>`

Used to allow seeking over previously read bytes on
non-seekable streams. This can be useful when transferring a non-seekable
entity body fails due to needing to rewind the stream (for example, resulting
from a redirect). Data that is read from the remote stream will be buffered in
a PHP temp stream so that previously read bytes are cached first in memory,
then on disk.

.. code-block:: php

    use GuzzleHttp\Psr7;

    $original = Psr7\stream_for(fopen('http://www.google.com', 'r'));
    $stream = new Psr7\CachingStream($original);

    $stream->read(1024);
    echo $stream->tell();
    // 1024

    $stream->seek(0);
    echo $stream->tell();
    // 0

InflateStream
-------------

:aws-php-class:`GuzzleHttp\\Psr7\\InflateStream </class-GuzzleHttp.Psr7.InflateStream.html>`

Uses PHP's zlib.inflate filter to inflate or deflate gzipped content.

This stream decorator skips the first 10 bytes of the given stream to remove
the gzip header, converts the provided stream to a PHP stream resource,
and then appends the zlib.inflate filter. The stream is then converted back
to a Guzzle stream resource to be used as a Guzzle stream.

LazyOpenStream
--------------

:aws-php-class:`GuzzleHttp\\Psr7\\LazyOpenStream </class-GuzzleHttp.Psr7.LazyOpenStream.html>`

Lazily reads or writes to a file that is opened only after an I/O operation
takes place on the stream.

.. code-block:: php

    use GuzzleHttp\Psr7;

    $stream = new Psr7\LazyOpenStream('/path/to/file', 'r');
    // The file has not yet been opened...

    echo $stream->read(10);
    // The file is opened and read from only when needed.

LimitStream
-----------

:aws-php-class:`GuzzleHttp\\Psr7\\LimitStream </class-GuzzleHttp.Psr7.LimitStream.html>`

Used to read a subset or slice of an existing stream object.
This can be useful for breaking a large file into smaller pieces to be sent in
chunks (e.g., the |S3| Multipart Upload API).

.. code-block:: php

    use GuzzleHttp\Psr7;

    $original = Psr7\stream_for(fopen('/tmp/test.txt', 'r+'));
    echo $original->getSize();
    // >>> 1048576

    // Limit the size of the body to 1024 bytes and start reading from byte 2048
    $stream = new Psr7\LimitStream($original, 1024, 2048);
    echo $stream->getSize();
    // >>> 1024
    echo $stream->tell();
    // >>> 0

NoSeekStream
------------

:aws-php-class:`GuzzleHttp\\Psr7\\NoSeekStream </class-GuzzleHttp.Psr7.NoSeekStream.html>`

Wraps a stream and does not allow seeking.

.. code-block:: php

    use GuzzleHttp\Psr7;

    $original = Psr7\stream_for('foo');
    $noSeek = new Psr7\NoSeekStream($original);

    echo $noSeek->read(3);
    // foo
    var_export($noSeek->isSeekable());
    // false
    $noSeek->seek(0);
    var_export($noSeek->read(3));
    // NULL

PumpStream
----------

:aws-php-class:`GuzzleHttp\\Psr7\\PumpStream </class-GuzzleHttp.Psr7.PumpStream.html>`

Provides a read-only stream that pumps data from a PHP callable.

When invoking the provided callable, the PumpStream will pass the amount of
data requested to read to the callable. The callable can choose to ignore
this value and return fewer or more bytes than requested. Any extra data
returned by the provided callable is buffered internally until drained using
the read() function of the PumpStream. The provided callable MUST return
false when there is no more data to read.

Implementing Stream Decorators
------------------------------

Creating a stream decorator is very easy thanks to the
:aws-php-class:`GuzzleHttp\\Psr7\\StreamDecoratorTrait
</class-GuzzleHttp.Psr7.StreamDecoratorTrait.html>`.
This trait provides methods that implement ``Psr\Http\Message\StreamInterface``
by proxying to an underlying stream. Just ``use`` the ``StreamDecoratorTrait``
and implement your custom methods.

For example, let's say we wanted to call a specific function each time the last
byte is read from a stream. This could be implemented by overriding the
``read()`` method.

.. code-block:: php

    use Psr\Http\Message\StreamInterface;
    use GuzzleHttp\Psr7\StreamDecoratorTrait;

    class EofCallbackStream implements StreamInterface
    {
        use StreamDecoratorTrait;

        private $callback;

        public function __construct(StreamInterface $stream, callable $cb)
        {
            $this->stream = $stream;
            $this->callback = $cb;
        }

        public function read($length)
        {
            $result = $this->stream->read($length);

            // Invoke the callback when EOF is hit
            if ($this->eof()) {
                call_user_func($this->callback);
            }

            return $result;
        }
    }

This decorator could be added to any existing stream and used like this.

.. code-block:: php

    use GuzzleHttp\Psr7;

    $original = Psr7\stream_for('foo');

    $eofStream = new EofCallbackStream($original, function () {
        echo 'EOF!';
    });

    $eofStream->read(2);
    $eofStream->read(1);
    // echoes "EOF!"
    $eofStream->seek(0);
    $eofStream->read(3);
    // echoes "EOF!"
