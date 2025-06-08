# CH19 -- check input stream is redirected to standard input stream
## objectives
You will learn how to

+ check input stream is redirected to standard input stream

## CH19.1 -- check input stream is redirected to standard input stream
### `Console.IsInputRedirected` getter-property

```
bool isInputRedirected = Console.IsInputRedirected;
```
 
will check input stream is redirected to standard input stream.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrates how to 
        /// 
        /// + redirect standard input, output, and error streams in the console application
        /// 
        /// + check input stream is redirected to standard input stream
        /// 
        /// + check output stream is redirected to standard output stream
        /// 
        /// + check error stream is redirected to standard error stream
        /// </summary>
        public static void TestMethod18()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("Is it redirected to standard input stream?{0}",Console.IsInputRedirected);
            Console.WriteLine("Is it redirected to standard output stream?{0}",Console.IsOutputRedirected);
            Console.WriteLine("Is it redirected to standard error stream?{0}",Console.IsErrorRedirected);

            Console.WriteLine("~~~~~~~~~~~~~~~~~~~~~~");

            string textContentFullPath = PathConstant.TEXT_CONTENT;

            string outputFullPath = FilePathHandler.FileFullPathHandler
                .GetExportedMessageFullPath(
                    MethodBase.GetCurrentMethod().Name ,
                    ".log"
                );

            string errorFullPath = FilePathHandler.FileFullPathHandler
                .GetExportedErrorMessageFullPath(
                    MethodBase.GetCurrentMethod().Name ,
                    ".log"
                );

            using(StreamReader streamReader = new StreamReader(textContentFullPath))
            {
                using(StreamWriter streamWriter = new StreamWriter(outputFullPath , true , Encoding.UTF8))
                {
                    using(StreamWriter errorStreamWriter = new StreamWriter(errorFullPath , true , Encoding.UTF8))
                    {
                        // Redirect standard error from the console to the error file.
                        Console.SetError(errorStreamWriter);
                        // Redirect standard output from the console to the output file.
                        Console.SetOut(streamWriter);
                        // Redirect standard input from the console to the input file.
                        Console.SetIn(streamReader);
                        Console.SetError(new StreamWriter(Console.OpenStandardError()));

                        Console.WriteLine("Output Message:\nHello World");
                        Console.WriteLine("Is it redirected to standard input stream?{0}" , Console.IsInputRedirected);
                        Console.WriteLine("Is it redirected to standard output stream?{0}" , Console.IsOutputRedirected);
                        Console.WriteLine("Is it redirected to standard error stream?{0}" , Console.IsErrorRedirected);

                        Console.WriteLine("~~~~~~~~~~~~~~~~~~~~~~");
                    }
                }
            }

            TextReader inputStandardStream = new StreamReader(Console.OpenStandardInput());
            TextWriter outputStandardStream = new StreamWriter(Console.OpenStandardOutput())
            {
                AutoFlush = true
            };
            TextWriter errorStandardStream = new StreamWriter(Console.OpenStandardError())
            {
                AutoFlush = true
            };

            Console.SetIn(inputStandardStream);
            Console.SetOut(outputStandardStream);
            Console.SetError(errorStandardStream);

            Console.WriteLine("Is it redirected to standard input stream?{0}" , Console.IsInputRedirected);
            Console.WriteLine("Is it redirected to standard output stream?{0}" , Console.IsOutputRedirected);
            Console.WriteLine("Is it redirected to standard error stream?{0}" , Console.IsErrorRedirected);

            Console.WriteLine("~~~~~~~~~~~~~~~~~~~~~~");

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod18 method call,
Is it redirected to standard input stream?False
Is it redirected to standard output stream?False
Is it redirected to standard error stream?False
~~~~~~~~~~~~~~~~~~~~~~
Is it redirected to standard input stream?False
Is it redirected to standard output stream?False
Is it redirected to standard error stream?False
~~~~~~~~~~~~~~~~~~~~~~
End of TestMethod18 method call,
```

Additionally, it will export output message into a `log` (`.log` file).

`Output in TestMethod18 method_f324bb75-47a2-432f-95ce-e132c64311fa`

```
Output Message:
Hello World
Is it redirected to standard input stream?False
Is it redirected to standard output stream?False
Is it redirected to standard error stream?False
~~~~~~~~~~~~~~~~~~~~~~

```

## reference
### API docs
+ [`Console.IsInputRedirected Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.isinputredirected?view=net-8.0)

+ [`Console.IsOutputRedirected Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.isoutputredirected?view=net-8.0)

+ [`Console.IsErrorRedirected Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.iserrorredirected?view=net-8.0#system-console-iserrorredirected)