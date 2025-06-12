# CH5 -- get or set encoding of stream writer
## objectives
You will learn how to

+ get encoding of stream writer

+ set encoding of stream writer

## CH5.1 -- get encoding of stream writer
### `Encoding` getter property

```
Encoding encoding = streamWriter.Encoding
```

will get the encoding of `streamWriter` instance.

#### Remarks
> [!CAUTION]
> Since `Encoding` is a getter property,
> 
> we can NOT set the encoding.

For example,

it will cause compile-time error

```
streamWriter.Encoding = Encoding.ASCII; // <-- can NOT assign it to `Encoding` getter property
```

If you want to do so, see CH5.2.

## CH5.2 -- set encoding of stream writer
### `StreamWriter` constructor
You can only set the encoding of stream writer when you instiantiate it 

(at the `encoding` parameter in the constructor).

```
using(
    StreamWriter streamWriter =
        new StreamWriter(
            outputFilePath ,
            false ,
            encoding: Encoding.Unicode
        )
)
{
    
}
```

sets the stream writer will write text into files with encoding `Unicode`.

## examples
### example 1
#### main code
Invoking following method will 

1. create directory `TestMethod5` under `~/AppData/DataSource/Output` directory (if it does NOT exist).

2. create file `output ex1.txt`, `output ex2.txt`, and `output ex3.txt` under `~/AppData/DataSource/Output/TestMethod5` (if it does NOT exist).

3. then write (,or overwrite) text into those files as follows

`output ex1.txt`

```
Current encoding is `Unicode`
Hello Nico,Current time is:`6/12/2025 1:39:28 PM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`

```

`output ex2.txt`

```
Current encoding is `Unicode (UTF-32)`
Hello Nico,Current time is:`6/12/2025 1:39:28 PM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`

```

`output ex3.txt`

```
Current encoding is `Unicode`
Hello Nico,Current time is:`6/12/2025 1:39:28 PM`
You want to dance at LoveLive club

Here is the first time to join the competition `LoveLive`

```

method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + write some text to a file with different encoding.
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string outputFilePath;
            string outputFileName;
            string formattingString;
            string testMethodName = MethodBase.GetCurrentMethod().Name;
            string testMethodDirectory = System.IO.Path.Combine(Constants.PathConstants.DirectoryConstants.OUTPUT , testMethodName);

            int status = DirectoryHandler.CreateDirectory(testMethodDirectory);
            if(status == -1)
            {
                return;
            }

            int counter = 1;
            formattingString = "output ex{0}.txt";

            ///* --------------- Example 1 --------------- *///
            Console.WriteLine("///* --------------- Example {0} --------------- *///",counter);
            outputFileName = string.Format(formattingString , counter);
            outputFilePath = System.IO.Path.Combine(testMethodDirectory , outputFileName);
            FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist

            using(
                StreamWriter streamWriter =
                    new StreamWriter(
                        outputFilePath ,
                        false ,
                        encoding: Encoding.Unicode
                    )
            )
            {
                streamWriter.WriteLine("Current encoding is `{0}`" , streamWriter.Encoding.EncodingName);

                streamWriter.Write("Hello Nico,");
                streamWriter.Write("Current time is:`{0}`" , DateTime.Now.ToString());
                streamWriter.Write('\n');
                streamWriter.WriteLine("You want to dance at LoveLive club");
                streamWriter.WriteLine();
                streamWriter.WriteLine("Here is the first time to join the competition `LoveLive`");
            }

            counter++;

            ///* --------------- Example 2 --------------- *///
            Console.WriteLine("///* --------------- Example {0} --------------- *///" , counter);
            outputFileName = string.Format(formattingString , counter);
            outputFilePath = System.IO.Path.Combine(testMethodDirectory , outputFileName);
            FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist

            using(
                StreamWriter streamWriter = 
                    new StreamWriter(
                        outputFilePath , 
                        false,
                        encoding:Encoding.UTF32
                    )
            )
            {
                streamWriter.WriteLine("Current encoding is `{0}`",streamWriter.Encoding.EncodingName);

                streamWriter.Write("Hello Nico,");
                streamWriter.Write("Current time is:`{0}`" , DateTime.Now.ToString());
                streamWriter.Write('\n');
                streamWriter.WriteLine("You want to dance at LoveLive club");
                streamWriter.WriteLine();
                streamWriter.WriteLine("Here is the first time to join the competition `LoveLive`");
            }

            counter++;

            ///* --------------- Example 3 --------------- *///
            Console.WriteLine("///* --------------- Example {0} --------------- *///" , counter);
            outputFileName = string.Format(formattingString , counter);
            outputFilePath = System.IO.Path.Combine(testMethodDirectory , outputFileName);
            FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist

            using(
                StreamWriter streamWriter =
                    new StreamWriter(
                        outputFilePath ,
                        false ,
                        encoding: new UnicodeEncoding()
                    )
            )
            {
                streamWriter.WriteLine("Current encoding is `{0}`" , streamWriter.Encoding.EncodingName);
                streamWriter.Write("Hello Nico,");
                streamWriter.Write("Current time is:`{0}`" , DateTime.Now.ToString());
                streamWriter.Write('\n');
                streamWriter.WriteLine("You want to dance at LoveLive club");
                streamWriter.WriteLine();
                streamWriter.WriteLine("Here is the first time to join the competition `LoveLive`");
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

## reference
### API docs
+ [`StreamWriter.Encoding Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.encoding?view=net-8.0)

+ [`StreamWriter Constructors`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.-ctor?view=net-8.0)