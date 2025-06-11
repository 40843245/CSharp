# CH12 -- clear internal buffer data of stream reader
## objectives
You will learn how to

+ clear internal buffer data of stream reader

## CH12 -- clear internal buffer data of stream reader
### `DiscardBufferedData` instance method
clears internal buffer data of stream reader.

## examples
### example 1
Invoking the following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + clear internal buffer of the `System.IO.StreamReader` 
        /// 
        /// using `System.IO.StreamReader.DiscardBufferedData` method.
        /// </summary>
        public static void TestMethod18()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            int numberOfCharToRead = 0;
            int index = 0;
            int count = 0;
            int length = 4; // Define the length of the buffer
            char [ ] buffer = new char [ length ]; // Allocate a buffer on the heap
            string restOfFileContent = string.Empty;

            int counter = 1;

            ///* ----------- Example 1 ----------- *///
            Console.WriteLine("///* ----------- Example {0} ----------- *///" , counter);
            using(StreamReader streamReader = new StreamReader(filePath))
            {
                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                buffer = new char [ length ];
                index = 0;
                count = buffer.Length;
                numberOfCharToRead = streamReader.Read(buffer , index , count);

                Console.WriteLine("read `{0}` char, these chars `{1}` to be read." , numberOfCharToRead , (new string(buffer)).Replace("\n" , "\\n").Replace("\r" , "\\r"));

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.DiscardBufferedData(); // Discard any buffered data

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                restOfFileContent = streamReader.ReadToEnd();
                Console.WriteLine("rest of file content:\n`{0}`" , restOfFileContent.Replace("\n" , "\\n").Replace("\r" , "\\r"));

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.DiscardBufferedData(); // Discard any buffered data

                Console.WriteLine("After discarding buffered data,");

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;
            }

            counter++;

            ///* ----------- Example 2 ----------- *///
            Console.WriteLine("///* ----------- Example {0} ----------- *///" , counter);
            using(StreamReader streamReader = new StreamReader(filePath))
            {
                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                buffer = new char [ length ];
                index = 0;
                count = buffer.Length;
                numberOfCharToRead = streamReader.Read(buffer , index , count);

                Console.WriteLine("read `{0}` char, these chars `{1}` to be read." , numberOfCharToRead , (new string(buffer)).Replace("\n" , "\\n").Replace("\r" , "\\r"));

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.DiscardBufferedData(); // Discard any buffered data

                Console.WriteLine("After discarding buffered data,");

                streamReader.BaseStream.Seek(2 , SeekOrigin.Begin);
                Console.WriteLine("Back to offset 2,");

                restOfFileContent = streamReader.ReadToEnd();
                Console.WriteLine("rest of file content:\n`{0}`" , restOfFileContent.Replace("\n" , "\\n").Replace("\r" , "\\r"));

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;

                streamReader.DiscardBufferedData(); // Discard any buffered data

                Console.WriteLine("After discarding buffered data,");

                Console.ForegroundColor = ConsoleColor.Green;

                Console.WriteLine("The length of the stream is `{0}`" , streamReader.BaseStream.Length);
                Console.WriteLine("The position of the stream is `{0}`" , streamReader.BaseStream.Position);
                Console.WriteLine("Is this stream end of stream? `{0}`" , streamReader.EndOfStream);

                Console.ForegroundColor = ConsoleColor.White;
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

[`Output of console in TestMethod18`](./Output%20of%20console%20in%20TestMethod18.docx)