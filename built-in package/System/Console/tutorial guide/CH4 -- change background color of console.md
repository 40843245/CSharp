# CH4 -- change background color of console
## objectives
You will learn how to

+ change background color of console to specific color

+ reset background to default color

## CH4.1 -- change background color of console to specific color
### `Console.BackgroundColor` static property

```
Console.BackgroundColor = ConsoleColor.Red
```

will set the background of console to red.

## CH4.2 -- reset background to default color
### `Console.ResetColor` static method

```
Console.ResetColor();
```

will reset background to default color.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illstrates how to change the console background color and reset it back to default.
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

            Console.BackgroundColor = ConsoleColor.Green;
            
            Console.WriteLine("This is a test message from {0} method call.", MethodBase.GetCurrentMethod().Name);

            Console.ResetColor();

            Console.WriteLine("This is another test message from {0} method call.", MethodBase.GetCurrentMethod().Name);

            Console.BackgroundColor = ConsoleColor.Red;

            Console.WriteLine("This is a test message with red background from {0} method call.", MethodBase.GetCurrentMethod().Name);

            Console.BackgroundColor = ConsoleColor.Black;

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

![BackgroundColor of Console](BackgroundColor%20of%20Console.png)

[output of TestMethod2 method call](output%20of%20TestMethod2%20method%20call.docx)

## reference
### API docs
+ [`Console.BackgroundColor Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.backgroundcolor?view=net-9.0#system-console-backgroundcolor)

+ [`ConsoleColor Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.consolecolor?view=net-9.0)

+ [`Console.Clear Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.clear?view=net-9.0)