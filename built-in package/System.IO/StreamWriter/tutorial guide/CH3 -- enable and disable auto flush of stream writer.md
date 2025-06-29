# CH3 -- enable and disable auto flush of stream writer
## objectives
You will learn how to

+ enable auto flush of stream writer

+ disable auto flush of stream writer

## CH3.1 -- enable auto flush of stream writer
sets setter-getter property `AutoFlush` of `StreamWriter` instance to `true`.

```
streamWriter.AutoFlush = true;
```

## CH3.2 -- disable auto flush of stream writer
sets setter-getter property `AutoFlush` of `StreamWriter` instance to `false`.

```
streamWriter.AutoFlush = false;
```

## CH3.3 -- `AutoFlush` setter-getter property
It sets/gets the stream can auto flush or not

```
bool IsAutoFlush = streamWriter.AutoFlush;
assert(IsAutoFlush);
```

indicates that

the stream will auto flush.

### Remarks
see Remarks in CH4.

## examples
### example 1
#### main code
Invoking following method will 

1. create directory `TestMethod3` under `~/AppData/DataSource/Output` directory (if it does NOT exist).

2. create file `outputSyncWithoutAutoFlushFilePath.txt` and `outputSyncWithAutoFlushFilePath.txt` under `~/AppData/DataSource/Output/TestMethod3` (if it does NOT exist).


3. then write (,or overwrite) text into those files as follows

`outputSyncWithoutAutoFlushFilePath.txt`

```
Hello Nico,Current time is:`6/12/2025 10:42:23 AM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`
I will sing a song named -- lovelive paradise.
Here is the sentence.
3,2,1! go!
3,2,1! go!
finsh of this song.
```

`outputSyncWithoutAutoFlushFilePath.txt`

```
Hello Nico,Current time is:`6/12/2025 10:42:23 AM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`
I will sing a song named -- lovelive paradise.
Here is the sentence.
3,2,1! go!
3,2,1! go!
finsh of this song.
```

method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + enable and disable auto flush of stream writer
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string testMethodName = MethodBase.GetCurrentMethod().Name;
            string testMethodDirectory = System.IO.Path.Combine(Constants.PathConstants.DirectoryConstants.OUTPUT , testMethodName);

            int status = DirectoryHandler.CreateDirectory(testMethodDirectory);
            if(status == -1)
            {
                return;
            }

            char [ ] charArray = new char [ ] { 'H' , 'e' , 'r' , 'e' , ' ' , 'i' , 's' , ' ' , 't' , 'h' , 'e' , ' ' , 's' , 'e' , 'n' , 't' , 'e' , 'n' , 'c' , 'e' , '.' };
            ReadOnlySpan<char> readOnlySpan = new ReadOnlySpan<char>(new char [ ] { '3' , ',' , '2' , ',' , '1' , '!' , ' ' , 'g' , 'o' , '!' });
            char [ ] charArray2 = new char [ ] { 'f' , 'i' , 'n' , 's' , 'h' , ' ' , 'o' , 'f' , ' ' , 't' , 'h' , 'i' , 's' , ' ' , 's' , 'o' , 'n' , 'g' , '.' };

            string outputSyncWithoutAutoFlushFilePath = System.IO.Path.Combine(testMethodDirectory , "outputSyncWithoutAutoFlushFilePath.txt");
            FileHandler.CreateFile(outputSyncWithoutAutoFlushFilePath); // Create the file if it does not exist

            using(StreamWriter streamWriter = new StreamWriter(outputSyncWithoutAutoFlushFilePath,false))
            {
                streamWriter.AutoFlush = false; // Disable auto-flush to demonstrate manual flushing

                streamWriter.Write("Hello Nico,");
                streamWriter.Write(string.Format("Current time is:`{0}`" , DateTime.Now.ToString()));
                streamWriter.Write('\n');
                streamWriter.WriteLine("You want to dance at LoveLive club");
                streamWriter.WriteLine();
                streamWriter.WriteLine("Here is the first time to join the competition `LoveLive`");
                streamWriter.WriteLine("I will sing a song named -- lovelive paradise.");
                for(int i = 0; i < charArray.Length; i++)
                {
                    streamWriter.Write(charArray [ i ]);
                }
                streamWriter.Write('\n');
                streamWriter.Write(readOnlySpan);
                streamWriter.Write('\n');
                streamWriter.WriteLine(readOnlySpan.ToArray() , 0 , readOnlySpan.ToArray().Length);
                streamWriter.Write(charArray2 , 0 , charArray2.Length);

                streamWriter.Flush(); // Explicitly flush the stream to ensure data is written to the file
            }

            string outputSyncWithAutoFlushFilePath = System.IO.Path.Combine(testMethodDirectory , "outputSyncWithAutoFlushFilePath.txt");
            FileHandler.CreateFile(outputSyncWithAutoFlushFilePath); // Create the file if it does not exist

            using(StreamWriter streamWriter = new StreamWriter(outputSyncWithAutoFlushFilePath , false))
            {

                streamWriter.AutoFlush = true; // Enable auto-flush to ensure data is written immediately

                streamWriter.Write("Hello Nico,");
                streamWriter.Write(string.Format("Current time is:`{0}`" , DateTime.Now.ToString()));
                streamWriter.Write('\n');
                streamWriter.WriteLine("You want to dance at LoveLive club");
                streamWriter.WriteLine();
                streamWriter.WriteLine("Here is the first time to join the competition `LoveLive`");
                streamWriter.WriteLine("I will sing a song named -- lovelive paradise.");
                for(int i = 0; i < charArray.Length; i++)
                {
                    streamWriter.Write(charArray [ i ]);
                }
                streamWriter.Write('\n');
                streamWriter.Write(readOnlySpan);
                streamWriter.Write('\n');
                streamWriter.WriteLine(readOnlySpan.ToArray() , 0 , readOnlySpan.ToArray().Length);
                streamWriter.Write(charArray2 , 0 , charArray2.Length);

                // streamWriter.Flush(); // we don't need to call Flush() explicitly since AutoFlush is true
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }

```

### example 2
#### main code
Invoking following method will 

1. create directory `TestMethod4` under `~/AppData/DataSource/Output` directory (if it does NOT exist).

2. create file `outputAsyncWithoutAutoFlushFilePath.txt` and `outputAsyncWithAutoFlushFilePath.txt` under `~/AppData/DataSource/Output/TestMethod4` (if it does NOT exist).


3. then write (,or overwrite) text into those files as follows

`outputSyncWithoutAutoFlushFilePath.txt`

```
Hello Nico,Current time is:`6/12/2025 12:55:10 PM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`
I will sing a song named -- lovelive paradise.
Here is the sentence.
3,2,1! go!
3,2,1! go!
finsh of this song.
```

`outputSyncWithoutAutoFlushFilePath.txt`

```
Hello Nico,Current time is:`6/12/2025 12:55:10 PM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`
I will sing a song named -- lovelive paradise.
Here is the sentence.
3,2,1! go!
3,2,1! go!
finsh of this song.
```

method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + enable and disable auto flush of stream writer
        /// </summary>
        public async static void TestMethod4()
        {
            string testMethodName = MethodHelper.GetCallerMethodName();
            Console.WriteLine("In {0} method call," , testMethodName);

            string testMethodDirectory = System.IO.Path.Combine(Constants.PathConstants.DirectoryConstants.OUTPUT , testMethodName);

            int status = DirectoryHandler.CreateDirectory(testMethodDirectory);
            if(status == -1)
            {
                return;
            }

            char [ ] charArray = new char [ ] { 'H' , 'e' , 'r' , 'e' , ' ' , 'i' , 's' , ' ' , 't' , 'h' , 'e' , ' ' , 's' , 'e' , 'n' , 't' , 'e' , 'n' , 'c' , 'e' , '.' };
            char [] charArry3 = { '3' , ',' , '2' , ',' , '1' , '!' , ' ' , 'g' , 'o' , '!' };
            char [ ] charArray2 = new char [ ] { 'f' , 'i' , 'n' , 's' , 'h' , ' ' , 'o' , 'f' , ' ' , 't' , 'h' , 'i' , 's' , ' ' , 's' , 'o' , 'n' , 'g' , '.' };

            string outputAsyncWithoutAutoFlushFilePath = System.IO.Path.Combine(testMethodDirectory , "outputAsyncWithoutAutoFlushFilePath.txt");
            FileHandler.CreateFile(outputAsyncWithoutAutoFlushFilePath); // Create the file if it does not exist

            using(StreamWriter streamWriter = new StreamWriter(outputAsyncWithoutAutoFlushFilePath , false))
            {
                streamWriter.AutoFlush = false; // Disable auto-flush to demonstrate manual flushing

                await streamWriter.WriteAsync("Hello Nico,");
                await streamWriter.WriteAsync(string.Format("Current time is:`{0}`" , DateTime.Now.ToString()));
                await streamWriter.WriteAsync('\n');
                await streamWriter.WriteLineAsync("You want to dance at LoveLive club");
                await streamWriter.WriteLineAsync();
                await streamWriter.WriteLineAsync("Here is the first time to join the competition `LoveLive`");
                await streamWriter.WriteLineAsync("I will sing a song named -- lovelive paradise.");
                for(int i = 0; i < charArray.Length; i++)
                {
                    await streamWriter.WriteAsync(charArray [ i ]);
                }
                await streamWriter.WriteAsync('\n');
                await streamWriter.WriteAsync(charArry3,0,charArry3.Length);
                await streamWriter.WriteAsync('\n');
                await streamWriter.WriteLineAsync(charArry3 , 0 , charArry3.Length);
                await streamWriter.WriteLineAsync(charArray2 , 0 , charArray2.Length);

                await streamWriter.FlushAsync(); // Explicitly flush the stream to ensure data is written to the file
            }

            string outputAsyncWithAutoFlushFilePath = System.IO.Path.Combine(testMethodDirectory , "outputAsyncWithAutoFlushFilePath.txt");
            FileHandler.CreateFile(outputAsyncWithAutoFlushFilePath); // Create the file if it does not exist

            using(StreamWriter streamWriter = new StreamWriter(outputAsyncWithAutoFlushFilePath , false))
            {
                streamWriter.AutoFlush = true; // Enable auto-flush to ensure data is written immediately

                await streamWriter.WriteAsync("Hello Nico,");
                await streamWriter.WriteAsync(string.Format("Current time is:`{0}`" , DateTime.Now.ToString()));
                await streamWriter.WriteAsync('\n');
                await streamWriter.WriteLineAsync("You want to dance at LoveLive club");
                await streamWriter.WriteLineAsync();
                await streamWriter.WriteLineAsync("Here is the first time to join the competition `LoveLive`");
                await streamWriter.WriteLineAsync("I will sing a song named -- lovelive paradise.");
                for(int i = 0; i < charArray.Length; i++)
                {
                    streamWriter.Write(charArray [ i ]);
                }
                await streamWriter.WriteAsync('\n');
                await streamWriter.WriteAsync(charArry3,0,charArry3.Length);
                await streamWriter.WriteAsync('\n');
                await streamWriter.WriteLineAsync(charArry3 , 0 , charArry3.Length);
                await streamWriter.WriteLineAsync(charArray2 , 0 , charArray2.Length);

                // await streamWriter.FlushAsync(); // we don't need to call FlushAsync() explicitly since AutoFlush is true
            }

            Console.WriteLine("End of {0} method call," , testMethodName);
        }
```

## reference
### API docs
+ [`StreamWriter.AutoFlush Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.autoflush?view=net-8.0)

+ [`StreamWriter.Flush Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.flush?view=net-8.0)

+ [`StreamWriter.FlushAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.flushasync?view=net-8.0)