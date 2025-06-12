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

the stream can auto flush.

### Remarks
+ If `AutoFlash` setter-getter property returns `false`,

then when the first operation of `StreamWriter` instance,

the operation of `StreamWriter` instance MUST be wait for 

`the first operation1, Otherwise, instance is thrown a rumtime 

error at runtime. 

And it need to manually flush it (through invoking `Flush` instance method).

+ If `AutoFlash` setter-getter property returns `true`,

then when the first operation of `StreamWriter` instance,

it will perform auto flush (through invoking `Flush` instance method)

the operation of `StreamWriter` instance MUST be wait for 

`the first operation1, Otherwise, instance is thrown a rumtime 

error at runtime. 

And it need not to manually flush it (through invoking `Flush` instance method).

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

## reference
### API docs

+ [`StreamWriter.AutoFlush Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.autoflush?view=net-8.0)

+ [`StreamWriter.Flush Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.flush?view=net-8.0)