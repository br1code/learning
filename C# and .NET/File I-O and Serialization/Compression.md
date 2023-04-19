# Compression

## Simple Explanation

Compression is a way to reduce the size of data by removing redundancy and compressing the data into a smaller form. In C# and .NET, you can use classes in the System.IO.Compression namespace to compress and decompress data.

## Deep Explanation

Compression is the process of reducing the size of data by removing redundancy and compressing the data into a smaller form. Compression is often used to reduce the amount of space that data takes up on disk or in memory, and to reduce the amount of time it takes to transfer data over a network.

In C# and .NET, you can use classes in the System.IO.Compression namespace to compress and decompress data. The most commonly used classes are GZipStream and DeflateStream, which implement the gzip and deflate compression algorithms, respectively.

To compress data, you first create a FileStream or MemoryStream to read the data from, and then create a GZipStream or DeflateStream to write the compressed data to. You then read the data from the input stream and write it to the output stream, and when you're done, you close both streams. To decompress data, you do the same thing in reverse: you create a FileStream or MemoryStream to write the decompressed data to, and then create a GZipStream or DeflateStream to read the compressed data from. You then read the data from the input stream and write it to the output stream, and when you're done, you close both streams.

## Examples

Here's a basic example of compressing and decompressing a file using GZipStream:

```C#
using System;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main(string[] args)
    {
        // Compress a file
        using (FileStream input = File.OpenRead("input.txt"))
        using (FileStream output = File.Create("output.gz"))
        using (GZipStream compressor = new GZipStream(output, CompressionMode.Compress))
        {
            input.CopyTo(compressor);
        }

        // Decompress the compressed file
        using (FileStream input = File.OpenRead("output.gz"))
        using (FileStream output = File.Create("output.txt"))
        using (GZipStream decompressor = new GZipStream(input, CompressionMode.Decompress))
        {
            decompressor.CopyTo(output);
        }
    }
}
```