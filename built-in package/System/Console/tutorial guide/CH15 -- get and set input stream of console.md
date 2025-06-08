# CH15 -- get and set input stream of console
## objectives
You will learn how to

+ get input stream of console

+ set input stream of console

## CH15.1 -- get input stream of console
### `Console.In` getter-property

```
TextReader textWriterInput = Console.In;
```

will return input stream of console.

By default, input stream of console is standard input stream.

```
TextReader textWriterInput = Console.In;
textWriterOutput.Write("What is your name: ");
String name = textWriterInput.ReadLine();
```

will print `What is your name: ` in console and 

wait until the user pressed the `Enter`.

## CH15.2 -- set input stream of console
### `Console.SetIn` static method

```
TextReader standardedInputStream = new StreamReader(Console.OpenStandardInput());
Console.SetIn(standardedInputStream);
```

will set the input stream as standard input stream.

## examples
### example 1
see example 1 in CH14.

### example 2
#### main code
The following example replaces four consecutive space characters in a string with a tab character.

Invoking following method

```
        /// <summary>
        /// The following example replaces four consecutive space characters in a string with a tab character.
        /// </summary>
        public static void TestMethod17()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            int tabSize = 4; // Define the tab size.
            string outputFilePath = FilePathHandler.FileFullPathHandler
                .GetExportedMessageFullPath(
                    MethodBase.GetCurrentMethod().Name ,
                    ".txt"
                );

            string inputFilePath = PathConstant.TEXT_CONTENT;

            using(var writer = new StreamWriter(outputFilePath))
            {
                using(var reader = new StreamReader(inputFilePath))
                {
                    // Redirect standard output from the console to the output file.
                    Console.SetOut(writer);
                    // Redirect standard input from the console to the input file.
                    Console.SetIn(reader);
                    string line;
                    while((line = Console.ReadLine()) != null)
                    {
                        string newLine = line.Replace(("").PadRight(tabSize , ' ') , "\t");
                        Console.WriteLine(newLine);
                    }
                }
            }

            TextWriter standardedOutputStream = new StreamWriter(Console.OpenStandardOutput())
            {
                AutoFlush = true
            };
            Console.SetOut(standardedOutputStream);

            TextReader standardedInputStream = new StreamReader(Console.OpenStandardInput());
            Console.SetIn(standardedInputStream);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod17 method call,
End of TestMethod17 method call,
```

## reference
### API docs
+ [`Console.In Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.in?view=net-8.0)

+ [`Console.SetIn(TextReader) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.setin?view=net-8.0)