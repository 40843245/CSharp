# CH1 -- write texts into a file using stream writer
## objectives
You will learn how to

+ write texts into a file using stream writer
## CH1.1 -- write texts into a file using stream writer
### `StreamWriter.Write` instance method
Relying on zero-based index,

+ ~~For zero arguments~~
+ For one argument:

append the text into stream writer.

+ For more arguments: 

```
public override void Write(string formattingString, params object?[] arg);
```

converts it one-by-one from all arguments except for zero argument to `object` type,

then formats they to zero argument `formattingString`

> [!CAUTION]
> `StreamWriter.Write` instance method can NOT accept zero argument.
>
> Invoking it with zero arguments will cause compile-time error.

#### Overloads
+ [`StreamWriter.Write Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.write?view=net-8.0)

### `StreamWriter.WriteLine` instance method
Behaves similar to `StreamWriter.Write` instance method.

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
+ [`StreamWriter.WriteLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.writeline?view=net-8.0)

## examples
### example 1
#### main code
Invoking following method will 

1. create directory `TestMethod1` under `~/AppData/DataSource/Output` directory (if it does NOT exist).

2. create file `output.txt` under `~/AppData/DataSource/Output/TestMethod1` (if it does NOT exist).

3. then write (,or overwrite) text into that file as follows

```
This is a test message from TestMethod1 method.
Hello Nico,Current time is:`6/12/2025 9:53:04 AM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`

```

```
/// <summary>
/// illustrate how to
/// 
/// + Write some text to a file in a specific file.
/// </summary>
public static void TestMethod1()
{
    Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

    string testMethodName = MethodBase.GetCurrentMethod().Name;
    string testMethodDirectory = System.IO.Path.Combine(Constants.PathConstants.DirectoryConstants.OUTPUT, testMethodName);
    
    int status = DirectoryHandler.CreateDirectory(testMethodDirectory);
    if(status == -1)
    {
        return;
    }

    string outputFilePath = System.IO.Path.Combine(testMethodDirectory, "output.txt");
    FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist
    using(StreamWriter streamWriter = new StreamWriter(outputFilePath , false))
    {
        streamWriter.WriteLine("This is a test message from {0} method.", MethodBase.GetCurrentMethod().Name);
        streamWriter.Write("Hello Nico,");
        streamWriter.Write("Current time is:`{0}`",DateTime.Now.ToString());
        streamWriter.Write('\n');
        streamWriter.WriteLine("You want to dance at LoveLive club");
        streamWriter.WriteLine();
        streamWriter.WriteLine("Here is the first time to join the competition `LoveLive`");

    }
    Console.WriteLine("End of {0} method call,",MethodBase.GetCurrentMethod().Name);
}
```

## reference
### API docs
+ [`StreamWriter Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter?view=net-8.0)

+ [`StreamWriter.Write Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.write?view=net-8.0)

+ [`StreamWriter.WriteLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.writeline?view=net-8.0)