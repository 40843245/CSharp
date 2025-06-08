# CH14 -- get and set standard output stream
## objectives
You will learn how to

+ get standard output stream

+ set standard output stream

## CH14.1 -- get standard output stream
### `Console.Out` getter-property
A [TextWriter](https://learn.microsoft.com/en-us/dotnet/api/system.io.textwriter?view=net-8.0) that represents the standard output stream.

## CH14.2 -- set standard output stream
### `Console.SetOut` static method
Sets the [Out](https://learn.microsoft.com/en-us/dotnet/api/system.console.out?view=net-8.0#system-console-out) property to target the [TextWriter](https://learn.microsoft.com/en-us/dotnet/api/system.io.textwriter?view=net-8.0) object.

## examples
### example 1
#### main code
Invoking folloing method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + read input from the console using `Console.In` and 
        /// 
        /// + write output to the console using `Console.Out`.
        /// </summary>
        public static void TestMethod15()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            TextReader textWriterInput = Console.In;
            TextWriter textWriterOutput = Console.Out;

            textWriterOutput.WriteLine("Hola Mundo!");
            textWriterOutput.Write("What is your name: ");
            String name = textWriterInput.ReadLine();

            textWriterOutput.WriteLine("Buenos Dias, {0}!" , name);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

might output following when you enter your name `Huang Jay`

```
In TestMethod15 method call,
Hola Mundo!
What is your name: Huang Jay
Buenos Dias, Huang Jay!
End of TestMethod15 method call,
```

### example 2
#### main code
Invoking following method

```
        /// <summary>
        /// Export console output info a log (`.log`) file under `~/AppData/Output/Console` directory.
        /// </summary>
        public static void TestMethod16()
        {
            string fileFullPath = 
                FilePathHandler.FileFullPathHandler
                .GetExportedMessageFullPath(
                    MethodBase.GetCurrentMethod().Name,
                    ".log"
                );

            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("The output on the console will be exported into a `.log` file.");
            Console.WriteLine("Press the key after it finishes will see output on the console that is exported into the `.log` file");
            Console.WriteLine("Please open the log file until the it finishes to write output into the log file.\nOtherwise, you will see incomplete output");
            Console.WriteLine("You can see these exported output at `{0}` file" , fileFullPath);
            Console.WriteLine("Waiting a while...");

            TextWriter originalConsoleOut = Console.Out;

            FileHandler.CreateFile(fileFullPath); // create the file if it does not exist.

            using(
                StreamWriter consoleStreamWriter =
                new StreamWriter(fileFullPath , true , Encoding.UTF8)
            )
            {
                Console.SetOut(consoleStreamWriter);

                Console.WriteLine("Output Message:\nHello World");
                
                consoleStreamWriter.Flush();
            }

            Console.SetOut(originalConsoleOut);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod16 method call,
The output on the console will be exported into a `.log` file.
Press the key after it finishes will see output on the console that is exported into the `.log` file
Please open the log file until the it finishes to write output into the log file.
Otherwise, you will see incomplete output
You can see these exported output at `D:\workspace\tutorial projects\built-in package\System\Console\Example\Example\AppData\Output\Console\Output in TestMethod16 method_e03ae51d-928b-4db6-87be-d238f81d1a6c.log` file
Waiting a while...
End of TestMethod16 method call,
```

And will export console output info a log (`.log`) file under `~/AppData/Output/Console` directory.

## reference
### API docs
+ [`Console.Out Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.out?view=net-8.0)

+ [`Console.SetOut`](https://learn.microsoft.com/en-us/dotnet/api/system.console.setout?view=net-8.0)