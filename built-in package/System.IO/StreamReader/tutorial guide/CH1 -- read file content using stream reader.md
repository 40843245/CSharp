# CH1 -- read file content using stream reader
## objectives
You will learn how to

+ read file content using stream reader

## CH1.1 -- read file content using stream reader
### `StreamReader` class

To read file content, we can use stream reader -- `StreamReader` class, as follows.

```
string filePath; // a string that contains file path (for existing file).
StreamReader streamReader = new StringReader(filePath);
```

There are many overloads, see [`StreamReader Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader?view=net-8.0) for more details.

#### Remarks
Don't forget to dispose it if it will NOT used.

+ `Close` instance method:

In earlier version of `C#`, we can only invoke `Close` instance method to close it (then dispose it).

```
StreamReader streamReader = new StringReader(filePath);
// ... operations
streamReader.Close();
```

> [!CAUTION]
> We can not invoke `Dispose` instance method due to protection level.
>
> The `Dispose` instance method has `protected` modifier.
>
> ```
> protected override void Dispose(bool disposing);
> ```

+ `using` block 

In later version of `C#`, we can NOT ONLY invoke `Close` instance method BUT ALSO define it in `using` block.

```
using(StreamReader streamReader = new StringReader(filePath))
{
    // ... operations.
}
```

For the instance defined in `using` block, it will be disposed 

after the execution of `using` block.

For more details, see [`using` block](/keyword/using/using.md#using-block)

#### Exceptions
> [!CAUTION]
> If the `StreamReader` reads file that does NOT exist, it will 
> 
> throw an exception `FileNotFoundException`. 

> [!CAUTION]
> If the `StreamReader` reads file (that exists) 
> 
> that there are no permission to access it , it will 
> 
> still throw an exception `FileNotFoundException`.

## examples
### example 1
The following example will read the file content from 

`~/AppData/DataSource/Input/Words.txt` into a stream reader.

The print it on console line-by-line.

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
/// + read the content with given file with `System.IO.StreamReader`.
/// </summary>
public static void TestMethod1()
{
    Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

    string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
    string fileName = "Words.txt";
    string filePath = System.IO.Path.Combine(inputDirectory, fileName);

    using(StreamReader streamReader = new StreamReader(filePath))
    {
        string? line = null;
        while ((line = streamReader.ReadLine()) != null)
        {
            Console.WriteLine(line);
        }
    }

    Console.WriteLine("End of {0} method call,", MethodBase.GetCurrentMethod().Name);
}
```

will output following

```
In TestMethod1 method call,
Hello
World
A
File
Contain
A
List
Of
Words
End of TestMethod1 method call,
```

## reference
### API docs
+ [`StreamReader Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader?view=net-8.0)

+ [`StreamReader.ReadLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readline?view=net-8.0)

+ [`StreamReader.Close Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.close?view=net-8.0)

+ [`StreamReader.Dispose(Boolean) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.dispose?view=net-8.0)