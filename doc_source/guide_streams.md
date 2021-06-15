# Streams in the AWS SDK for PHP Version 3<a name="guide_streams"></a>

As part of its integration of the [PSR\-7](http://www.php-fig.org/psr/psr-7/) HTTP message standard, the AWS SDK for PHP uses the [PSR\-7 StreamInterface](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Psr.Http.Message.StreamInterface.html) internally as its abstraction over [PHP streams](http://php.net/manual/en/intro.stream.php)\. Any command with an input field defined as a blob, such as the `Body` parameter on an [S3::PutObject command](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#putobject), can be satisfied with a string, a PHP stream resource, or an instance of `Psr\Http\Message\StreamInterface`\.

**Warning**  
The SDK takes ownership of any raw PHP stream resource supplied as an input parameter to a command\. The stream is consumed and closed on your behalf\.  
If you need to share a stream between an SDK operation and your code, wrap it in an instance of `GuzzleHttp\Psr7\Stream` before including it as a command parameter\. The SDK consumes the stream, so your code needs to account for movement of the stream’s internal cursor\. Guzzle streams call `fclose` on the underlying stream resource when they are destroyed by PHP’s garbage collector, so you do not need to close the stream yourself\.

## Stream Decorators<a name="stream-decorators"></a>

Guzzle provides several stream decorators that you can use to control how the SDK and Guzzle interact with the streaming resource provided as an input parameter to a command\. These decorators can modify how handlers are able to read and seek on a given stream\. The following is a partial list; more can be found on the [GuzzleHttpPsr7 repository](https://github.com/guzzle/psr7)\.

### AppendStream<a name="appendstream"></a>

 [GuzzleHttp\\Psr7\\AppendStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.AppendStream.html) 

Reads from multiple streams, one after the other\.

```
use GuzzleHttp\Psr7;

$a = Psr7\stream_for('abc, ');
$b = Psr7\stream_for('123.');
$composed = new Psr7\AppendStream([$a, $b]);

$composed->addStream(Psr7\stream_for(' Above all listen to me'));

echo $composed(); // abc, 123. Above all listen to me.
```

### CachingStream<a name="cachingstream"></a>

 [GuzzleHttp\\Psr7\\CachingStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.CachingStream.html) 

Used to allow seeking over previously read bytes on non\-seekable streams\. This can be useful when transferring a non\-seekable entity body fails due to needing to rewind the stream \(for example, resulting from a redirect\)\. Data that is read from the remote stream is buffered in a PHP temp stream so that previously read bytes are cached first in memory, then on disk\.

```
use GuzzleHttp\Psr7;

$original = Psr7\stream_for(fopen('http://www.google.com', 'r'));
$stream = new Psr7\CachingStream($original);

$stream->read(1024);
echo $stream->tell();
// 1024

$stream->seek(0);
echo $stream->tell();
// 0
```

### InflateStream<a name="inflatestream"></a>

 [GuzzleHttp\\Psr7\\InflateStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.InflateStream.html) 

Uses PHP’s zlib\.inflate filter to inflate or deflate gzipped content\.

This stream decorator skips the first 10 bytes of the given stream to remove the gzip header, converts the provided stream to a PHP stream resource, and then appends the zlib\.inflate filter\. The stream is then converted back to a Guzzle stream resource to be used as a Guzzle stream\.

### LazyOpenStream<a name="lazyopenstream"></a>

 [GuzzleHttp\\Psr7\\LazyOpenStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.LazyOpenStream.html) 

Lazily reads or writes to a file that is opened only after an I/O operation takes place on the stream\.

```
use GuzzleHttp\Psr7;

$stream = new Psr7\LazyOpenStream('/path/to/file', 'r');
// The file has not yet been opened...

echo $stream->read(10);
// The file is opened and read from only when needed.
```

### LimitStream<a name="limitstream"></a>

 [GuzzleHttp\\Psr7\\LimitStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.LimitStream.html) 

Used to read a subset or slice of an existing stream object\. This can be useful for breaking a large file into smaller pieces to be sent in chunks \(e\.g\., the Amazon S3 Multipart Upload API\)\.

```
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
```

### NoSeekStream<a name="noseekstream"></a>

 [GuzzleHttp\\Psr7\\NoSeekStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.NoSeekStream.html) 

Wraps a stream and does not allow seeking\.

```
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
```

### PumpStream<a name="pumpstream"></a>

 [GuzzleHttp\\Psr7\\PumpStream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.PumpStream.html) 

Provides a read\-only stream that pumps data from a PHP callable\.

When invoking the provided callable, the PumpStream passes the amount of data requested to read to the callable\. The callable can choose to ignore this value and return fewer or more bytes than requested\. Any extra data returned by the provided callable is buffered internally until drained using the read\(\) function of the PumpStream\. The provided callable MUST return false when there is no more data to read\.

### Implementing Stream Decorators<a name="implementing-stream-decorators"></a>

Creating a stream decorator is very easy thanks to the [GuzzleHttp\\Psr7\\StreamDecoratorTrait](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-GuzzleHttp.Psr7.StreamDecoratorTrait.html)\. This trait provides methods that implement `Psr\Http\Message\StreamInterface` by proxying to an underlying stream\. Just `use` the `StreamDecoratorTrait` and implement your custom methods\.

For example, let’s say we wanted to call a specific function each time the last byte is read from a stream\. This could be implemented by overriding the `read()` method\.

```
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
```

This decorator could be added to any existing stream and used like this\.

```
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
```