# CH13 -- set error stream of console
## objectives
You will learn how to

+ set error stream of console

+ get error stream of console

## CH13.1 -- set error stream of console
### `Console.SetError` static method
Sets the [Error](https://learn.microsoft.com/en-us/dotnet/api/system.console.error?view=net-8.0#system-console-error) property to the specified [TextWriter](https://learn.microsoft.com/en-us/dotnet/api/system.io.textwriter?view=net-8.0) object.

## CH13.2 -- get error stream of console
### `Console.Error` static method
Gets the standard error output stream.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate
        /// 
        /// + set error stream of console through `Console.SetError()` method 
        /// 
        /// + get error stream of console through `Console.Error`
        /// </summary>
        public static void TestMethod14()
        {
            Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

            string [ ] args = Environment.GetCommandLineArgs();
            string errorOutput = "";
            // Make sure that there is at least one command line argument.
            if(args.Length <= 1)
            {
                errorOutput += "You must include a filename on the command line.\n";
            }

            string filePullPath = PathConstant.TEXT_CONTENT;
            // Display the contents of the file.
            StreamReader streamReader = new StreamReader(filePullPath);
            string contents = streamReader.ReadToEnd();
            streamReader.Close();
            Console.WriteLine(
                "*****Contents of file '{0}':\n\n" ,
                filePullPath
            );
            Console.WriteLine(contents);
            Console.WriteLine("*****\n");

            string logFile = PathConstant.LOG_GUID_FULLPATH;
            // Write error information to a file.
            Console.SetError(new StreamWriter(logFile));
            Console.Error.WriteLine(errorOutput);
            Console.Error.Close();
            // Reacquire the standard error stream.
            StreamWriter standardError = new StreamWriter(Console.OpenStandardError());
            standardError.AutoFlush = true;
            Console.SetError(standardError);
            Console.Error.WriteLine("\nError information written to '{0}'", logFile);
            standardError.Close();

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

might output following if executing it by clicking playing button.

```
In TestMethod14 method call,
*****Contents of file 'D:\workspace\tutorial projects\built-in package\System\Console\Example\Example\AppData\Console\text content.txt':


Hello World
Hello C#
Hello Java
Hello JS
Hello C++
*****


Error information written to 'D:\workspace\tutorial projects\built-in package\System\Console\Example\Example\AppData\Console\44c69e7b-d280-41ac-a539-f165fdb99545_log.txt'
End of TestMethod14 method call,
```

## reference 
### API docs
+ [`Console.SetError(TextWriter) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.seterror?view=net-8.0)

+ [`Console.Error Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.error?view=net-8.0)

+ [`TextWriter Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.textwriter?view=net-8.0)

+ [`StreamWriter Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter?view=net-8.0)