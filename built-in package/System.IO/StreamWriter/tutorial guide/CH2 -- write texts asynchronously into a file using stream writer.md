# CH1 -- write texts asynchronously into a file using stream writer
## objectives
You will learn how to

+ write texts asynchronously into a file using stream writer
## CH1.1 -- write texts asynchronously into a file using stream writer
### `StreamWriter.WriteAsync` instance method
Relying on zero-based index,

write texts asynchronously into a file using stream writer

> [!CAUTION]
> `StreamWriter.WriteAsync` instance method can NOT accept zero argument.
>
> Invoking it with zero arguments will cause compile-time error.

#### Overloads
+ [`StreamWriter.WriteAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.writeasync?view=net-8.0)

### `StreamWriter.WriteLineAsync` instance method
Behaves similar to `StreamWriter.WriteLineAsync` instance method.

However, it will append a new break line `\n`.

Additionally, it accepts zero argument.

Relying on zero-based index,

+ For zero arguments:

appends a new break line `\n`.

> [!TIP]
> Think of it:
>
> Their relationship is similar to the relationship between `Console.Write` and `Console.WriteLine`.

#### Overloads
+ [`StreamWriter.WriteLineAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.writelineasync?view=net-8.0)

## examples
### example 1
#### main code
Invoking following method will 

1. create directory `TestMethod2` under `~/AppData/DataSource/Output` directory (if it does NOT exist).

2. create file `output.txt` under `~/AppData/DataSource/Output/TestMethod2` (if it does NOT exist).

3. then write (,or overwrite) text into that file as follows

```
This is a test message from TestMethod2 method.
Hello Nico,Current time is:`6/12/2025 10:18:00 AM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`
I will sing a song named -- lovelive paradise.
Here is the sentence.
3,2,1! go!
3,2,1! go!
finsh of this song.
```

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + **asynchronously** write some text to a file in a specific file.
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string testMethodName = MethodBase.GetCurrentMethod().Name;
            string testMethodDirectory = System.IO.Path.Combine(Constants.PathConstants.DirectoryConstants.OUTPUT , testMethodName);

            int status = DirectoryHandler.CreateDirectory(testMethodDirectory);
            if(status == -1)
            {
                return;
            }

            string outputFilePath = System.IO.Path.Combine(testMethodDirectory , "output.txt");
            FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist

            char [ ] charArray = new char [ ] { 'H' , 'e' , 'r' , 'e' , ' ' , 'i' , 's' , ' ' , 't' , 'h' , 'e' , ' ' , 's' , 'e' , 'n' , 't' , 'e' , 'n' , 'c' , 'e' , '.' };
            ReadOnlyMemory<char> readOnlyMemory = new ReadOnlyMemory<char>(new char [ ] { '3' , ',' , '2' , ',' , '1' , '!' , ' ' , 'g' , 'o' , '!' });
            char [] charArray2 = new char [ ] { 'f' , 'i' , 'n' , 's' , 'h' , ' ' , 'o' , 'f' , ' ', 't','h','i','s',' ','s','o','n','g','.' };
            using(StreamWriter streamWriter = new StreamWriter(outputFilePath , false))
            {
                streamWriter.WriteLineAsync(string.Format("This is a test message from {0} method." , MethodBase.GetCurrentMethod().Name));
                streamWriter.WriteAsync("Hello Nico,");
                streamWriter.WriteAsync(string.Format("Current time is:`{0}`" , DateTime.Now.ToString()));
                streamWriter.WriteAsync('\n');
                streamWriter.WriteLineAsync("You want to dance at LoveLive club");
                streamWriter.WriteLineAsync();
                streamWriter.WriteLineAsync("Here is the first time to join the competition `LoveLive`");
                streamWriter.WriteLineAsync("I will sing a song named -- lovelive paradise.");
                for(int i = 0; i < charArray.Length; i++)
                {
                    streamWriter.WriteAsync(charArray [ i ]);
                }
                streamWriter.WriteAsync('\n');
                streamWriter.WriteAsync(readOnlyMemory);
                streamWriter.WriteAsync('\n');
                streamWriter.WriteLineAsync(readOnlyMemory.ToArray(),0, readOnlyMemory.ToArray().Length);
                streamWriter.WriteAsync(charArray2,0,charArray2.Length);
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

## reference
### API docs
+ [`StreamWriter Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter?view=net-8.0)

+ [`StreamWriter.WriteAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.writeasync?view=net-8.0)

+ [`StreamWriter.WriteLineAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.writelineasync?view=net-8.0)