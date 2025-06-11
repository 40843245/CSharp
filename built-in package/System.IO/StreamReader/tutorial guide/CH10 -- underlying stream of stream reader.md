# CH10 -- underlying stream of stream reader
## objectives
You will learn how to

+ access underlying stream of stream reader

## CH10.1 -- underlying stream of stream reader
### `BaseStream` getter property
returns the underlying stream.

#### Remarks
If you manipulate the position of the underlying stream 

after reading data into the buffer, 

the position of the underlying stream might not match 

the position of the internal buffer. 

To reset the internal buffer, call the [DiscardBufferedData](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.discardbuffereddata?view=net-8.0) method.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + get underlying stream information using `System.IO.StreamReader.BaseStream` instance getter-property.
        /// 
        /// + get the position, length, read/write capabilities of the underlying stream
        /// 
        /// + seek the underlying stream using `System.IO.Stream.Seek` method
        /// 
        /// + etc.
        /// </summary>
        public static void TestMethod16()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                int nextCharCode = default;
                char nextChar = default;
                string restOfFileContent = default;

                Console.WriteLine("Can this stream can be read? `{0}`" , streamReader.BaseStream.CanRead);
                Console.WriteLine("Can this stream can be seeked? `{0}`" , streamReader.BaseStream.CanSeek);
                Console.WriteLine("Can this stream can be written? `{0}`" , streamReader.BaseStream.CanWrite);

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                nextCharCode = streamReader.Read();
                nextChar = (char)nextCharCode;

                Console.WriteLine("next char:`{0}`, next char code:`{1}`" , nextChar.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r") , nextCharCode);

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                restOfFileContent = streamReader.ReadToEnd();

                Console.WriteLine("rest of file content:\n{0}" , restOfFileContent.Replace("\n" , "\\n").Replace("\r" , "\\r"));

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.BaseStream.Seek(2 , SeekOrigin.Begin);

                restOfFileContent = streamReader.ReadToEnd();

                Console.WriteLine("rest of file content:\n{0}" , restOfFileContent.Replace("\n" , "\\n").Replace("\r" , "\\r"));

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.BaseStream.Seek(0 , SeekOrigin.Begin);

                nextCharCode = streamReader.Read();
                nextChar = (char)nextCharCode;

                Console.WriteLine("next char:`{0}`, next char code:`{1}`" , nextChar.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r") , nextCharCode);

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.BaseStream.Seek(5 , SeekOrigin.Current);

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

[`Output of console in TestMethod16.docx`](./Output%20of%20console%20in%20TestMethod16.docx)

## reference
### API docs
+ [`StreamReader.BaseStream Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.basestream?view=net-8.0)

+ [`Stream Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.stream?view=net-8.0)