# CH7 -- read charactes asynchronously from stream reader
## objectives
You will learn how to

+ read charactes asynchronously from stream reader

## CH7.1 -- read charactes asynchronously from stream reader
### `ReadAsync` instance method
****asynchronously**** reads one character or a set of character from position 

(that current pointer points to) and 

advance the position by the number of characters are read.

#### Overloads

+ `ReadAsync(Memory<char> buffer, System.Threading.CancellationToken cancellationToken = default)`: 

**asynchronously** reads the characters from the current stream 

into a memory block.

then returns `ValueTask<int>` instance with result -- 

the number of characters ar read, or 

returns `ValueTask<int>` instance with result 0 

iff the pointer points to the end of the stream and

there are no available char that can be read.

+ `ReadAsync(char[] buffer, int index, int count)`:

**asynchronous** reads `count` characters from position 

(that current pointer points to) plus offset `index` and 

advance the position by the number of characters to be read

then returns `Task<int>` with result -- 

number of characters to be read, or

returns `Task<int>` with result 0 

iff the pointer points to the end of the stream and

there are no available char that can be read.

### `ReadBlockAsync` instance method
**asynchronously** reads one character or a set of character **with guarantee** from position 

(that current pointer points to) and 

advance the position by the number of characters are read.

#### Overloads

+ `ReadBlockAsync(Memory<char> buffer, System.Threading.CancellationToken cancellationToken = default)`: 

**asynchronously** reads the characters **with guarantee** from the current stream 

into a memory block.

then returns `ValueTask<int>` instance with result -- 

the number of characters ar read, or 

returns `ValueTask<int>` instance with result 0 

iff the pointer points to the end of the stream and

there are no available char that can be read.

+ `ReadBlockAsync(char[] buffer, int index, int count)`:

**asynchronous** reads `count` characters **with guarantee** from position 

(that current pointer points to) plus offset `index` and 

advance the position by the number of characters to be read

then returns `Task<int>` with result -- 

number of characters to be read, or

returns `Task<int>` with result 0 

iff the pointer points to the end of the stream and

there are no available char that can be read.

### difference between `StreamReader.Read` and `StreamReader.ReadAsync`
For their behaviour, the main difference between

`StreamReader.Read(char[] buffer, int index, int count)`

and 

`StreamReader.ReadAsync(char[] buffer, int index, int count)`

instance method is 

the reading is **sync** or **async**

+ `StreamReader.Read(char[] buffer, int index, int count)`:

**synchronously** read more characters from stream reader.

+ `StreamReader.ReadAsync(char[] buffer, int index, int count)`

**asynchronously** read more characters from stream reader.

## examples
### example 1
#### main code
Assume `Words.txt` contains these text.

```
Hello
World
A
File
Contain
A
List
Of
Words
```

Invoking following method

```
       /// <summary>
       /// illustrate how to
       /// 
       /// + read more characters asynchronously from a file using `System.IO.StreamReader` and `TaskHelper.RunWithTimeout`.
       /// </summary>
       public static void TestMethod9()
       {
           Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
           string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
           string fileName = "Words.txt";
           string filePath = System.IO.Path.Combine(inputDirectory , fileName);

           int length = 10; // Define the length of the buffer
           int index = 0;
           int count = 0;
           char [ ] buffer = new char [length]; // Allocate a buffer on the heap

           int nextCharacterToRead = 0;
           Task<int>? readTask = null;

           count = buffer.Length;
           using(StreamReader streamReader = new StreamReader(filePath))
           {
               readTask = TaskHelper.RunWithTimeout<int>(
                   () => streamReader.ReadAsync(buffer,index,count) , 
                   TimeSpan.FromSeconds(5)
               ); // Wait for the task to complete
               while(readTask != null)
               {
                   nextCharacterToRead = readTask.Result;
                   if(!(nextCharacterToRead > 0))
                   {
                       break;
                   }
                   Console.WriteLine("read `{0}` char, these chars `{1}` to be read." , nextCharacterToRead , (new string(buffer)).Replace("\n" , "\\n").Replace("\r" , "\\r"));
                   readTask = TaskHelper.RunWithTimeout<int>(
                         () => streamReader.ReadAsync(buffer , index , count) ,
                        TimeSpan.FromSeconds(5)
                   ); // Wait for the task to complete
               }
           }
           Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
       }
```

will output following

```
In TestMethod9 method call,
read `10` char, these chars `Hello\r\nWor` to be read.
read `10` char, these chars `ld\r\nA\r\nFil` to be read.
read `10` char, these chars `e\r\nContain` to be read.
read `10` char, these chars `\r\nA\r\nList\r` to be read.
read `10` char, these chars `\nOf\r\nWords` to be read.
End of TestMethod9 method call,
```

### example 2
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + **asynchronously** read a set of character **with guarantee** from a file using `System.IO.StreamReader` and `Span<char>`.
        /// 
        /// </summary>
        public static void TestMethod13()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            int length = 10; // Define the length of the buffer
            char [] buffer = new char [ length ]; // Allocate a buffer on the heap
            Array.Fill(buffer,' ');
            Memory<char> memoryBuffer = new Memory<char>(buffer); // Create a Memory<char> from the char array

            int numberOfCharToRead = -1;
            ValueTask<int> readBlockValueTask;

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                readBlockValueTask = ValueTaskHelper.RunWithTimeout<int>(
                    () => streamReader.ReadBlockAsync(memoryBuffer) ,
                    TimeSpan.FromSeconds(5)
                ); // Wait for the task to complete
                numberOfCharToRead = readBlockValueTask.AsTask().Result;

                while(numberOfCharToRead > 0)
                {
                    Console.WriteLine("read `{0}` char, these chars `{1}` to be read." , numberOfCharToRead , memoryBuffer.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r"));

                    buffer = new char [ length ]; // Allocate a buffer on the heap
                    Array.Fill(buffer , ' ');
                    memoryBuffer = new Memory<char>(buffer); // Create a Memory<char> from the char array

                    readBlockValueTask = ValueTaskHelper.RunWithTimeout<int>(
                        () => streamReader.ReadBlockAsync(memoryBuffer) ,
                        TimeSpan.FromSeconds(5)
                    ); // Wait for the task to complete
                    numberOfCharToRead = readBlockValueTask.AsTask().Result;
                }
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod13 method call,
read `10` char, these chars `Hello\r\nWor` to be read.
read `10` char, these chars `ld\r\nA\r\nFil` to be read.
read `10` char, these chars `e\r\nContain` to be read.
read `10` char, these chars `e\r\nContain` to be read.
read `10` char, these chars `\r\nA\r\nList\r` to be read.
read `10` char, these chars `\nOf\r\nWords` to be read.
End of TestMethod13 method call,
```

### example 3
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + **asynchronously** read a set of character **with guarantee** from a file using `System.IO.StreamReader` and `char []`.
        /// </summary>
        public static void TestMethod14()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                int length = 10; // Define the length of the buffer
                int index = 0;
                int count = 0;
                int numberOfCharToRead = 0;
                char [ ] buffer = new char [ length ];
                Task<int> readBlockTask = default;

                for(
                    count = buffer.Length,
                    readBlockTask = TaskHelper.RunWithTimeout<int>(
                        () => streamReader.ReadBlockAsync(buffer , index , count) ,
                        TimeSpan.FromSeconds(5)
                    ), // Wait for the task to complete
                    numberOfCharToRead = readBlockTask.Result;
                    numberOfCharToRead > 0;
                    count = buffer.Length,
                    readBlockTask = TaskHelper.RunWithTimeout<int>(
                        () => streamReader.ReadBlockAsync(buffer , index , count) ,
                        TimeSpan.FromSeconds(5)
                    ), // Wait for the task to complete
                    numberOfCharToRead = readBlockTask.Result
                )
                {
                    Console.WriteLine("read `{0}` char, these chars `{1}` to be read." , numberOfCharToRead , (new string(buffer)).Replace("\n" , "\\n").Replace("\r" , "\\r"));
                }
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod14 method call,
read `10` char, these chars `Hello\r\nWor` to be read.
read `10` char, these chars `ld\r\nA\r\nFil` to be read.
read `10` char, these chars `e\r\nContain` to be read.
read `10` char, these chars `\r\nA\r\nList\r` to be read.
read `10` char, these chars `\nOf\r\nWords` to be read.
End of TestMethod14 method call,
```

## reference
### API docs
+ [`StreamReader.ReadAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readasync?view=net-8.0)

+ [`StreamReader.ReadBlockAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readblockasync?view=net-8.0)