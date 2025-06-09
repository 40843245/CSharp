# CH5 -- change foreground color of console
## objectives
You will learn how to

+ change foreground color of console to specific color

+ reset foreground to default color

## CH5.1 -- change foreground color of console to specific color
### `Console.BackgroundColor` static property

```
Console.BackgroundColor = ConsoleColor.Red
```

will set the foreground of console to red.

And the text will be highlighted with red.

## CH5.2 -- reset foreground to default color
### `Console.ResetColor` static method

```
Console.ResetColor();
```

will reset foreground to default color.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to change the console foreground color and reset it back to default.
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call,", MethodBase.GetCurrentMethod().Name);

            Console.ForegroundColor = ConsoleColor.Green;

            Console.WriteLine("This is a test message from {0} method call." , MethodBase.GetCurrentMethod().Name);

            Console.ResetColor();

            Console.WriteLine("This is another test message from {0} method call." , MethodBase.GetCurrentMethod().Name);

            Console.ForegroundColor = ConsoleColor.Red;

            Console.WriteLine("This is a test message with red foreground from {0} method call." , MethodBase.GetCurrentMethod().Name);

            Console.ForegroundColor = ConsoleColor.White;

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

![ForegroundColor of Console](ForegroundColor%20of%20Console.png)

[output of TestMethod3 method call](output%20of%20TestMethod3%20method%20call.docx)

## reference
### API docs
+ [`Console.ForegroundColor Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.foregroundcolor?view=net-9.0#system-console-foregroundcolor)

+ [`ConsoleColor Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.consolecolor?view=net-9.0)

+ [`Console.Clear Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.clear?view=net-9.0)