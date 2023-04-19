File I/O: This refers to reading and writing data to and from files on disk. The File and FileInfo classes provide methods for working with files, such as creating, opening, deleting, and moving files.

# File I/O

## Explanation

To read from or write to a file in C#, you first need to create a stream. A stream is a sequence of bytes that you can read from or write to. In .NET, there are several types of streams, such as FileStream, MemoryStream, and NetworkStream.

To create a FileStream, you can use the FileStream class and provide the path of the file and the mode in which you want to open it (read, write, or both). Here's an example:

```C#
string path = "C:\\example.txt";
FileStream fileStream = new FileStream(path, FileMode.Open, FileAccess.Read);
```

This code creates a FileStream for the file at the specified path and opens it in read mode.

Once you have a stream, you can read from or write to it using the Read and Write methods. Here's an example of reading from a file:

```C#
byte[] buffer = new byte[1024];
int bytesRead = fileStream.Read(buffer, 0, buffer.Length);
```

This code reads up to 1024 bytes from the file stream into a buffer array and returns the number of bytes that were read.

To write to a file, you can use the Write method. Here's an example:

```C#
byte[] data = Encoding.UTF8.GetBytes("Hello, world!");
fileStream.Write(data, 0, data.Length);
```

This code writes the string "Hello, world!" to the file stream in UTF-8 encoding.

Finally, when you're finished reading from or writing to a file, you should close the stream using the Close method. Here's an example:

```C#
fileStream.Close();
```

This code closes the file stream and releases any system resources that were being used by it.

## Examples

Here's a basic example of reading from and writing to a file:

```C#
string path = "C:\\example.txt";

// Write to file
byte[] data = Encoding.UTF8.GetBytes("Hello, world!");
using (FileStream fileStream = new FileStream(path, FileMode.Create, FileAccess.Write))
{
    fileStream.Write(data, 0, data.Length);
}

// Read from file
byte[] buffer = new byte[1024];
using (FileStream fileStream = new FileStream(path, FileMode.Open, FileAccess.Read))
{
    int bytesRead = fileStream.Read(buffer, 0, buffer.Length);
    string text = Encoding.UTF8.GetString(buffer, 0, bytesRead);
    Console.WriteLine(text);
}
```

---

To perform asynchronous file I/O operations, you can use the ReadAsync and WriteAsync methods of the FileStream class. These methods return a Task<int> object, which represents the asynchronous operation. You can await this task to wait for the operation to complete.

Here is an example of writing to a file asynchronously:

```C#
using System.IO;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        string text = "Hello, world!";

        using (FileStream fileStream = new FileStream("example.txt", FileMode.Create, FileAccess.Write, FileShare.None, bufferSize: 4096, useAsync: true))
        {
            byte[] buffer = Encoding.UTF8.GetBytes(text);
            await fileStream.WriteAsync(buffer, 0, buffer.Length);
        }
    }
}
```

In this example, we create a FileStream object with the useAsync parameter set to true, which enables asynchronous I/O. We then convert the string text to a byte array and write it to the file using the WriteAsync method. The await keyword is used to wait for the write operation to complete before continuing with the rest of the program.

Similarly, you can read from a file asynchronously using the ReadAsync method. Here's an example:

```C#
using System.IO;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        using (FileStream fileStream = new FileStream("example.txt", FileMode.Open, FileAccess.Read, FileShare.Read, bufferSize: 4096, useAsync: true))
        {
            byte[] buffer = new byte[4096];
            int bytesRead = await fileStream.ReadAsync(buffer, 0, buffer.Length);
            string text = Encoding.UTF8.GetString(buffer, 0, bytesRead);
            Console.WriteLine(text);
        }
    }
}
```