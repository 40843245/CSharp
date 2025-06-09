# CH3 -- wait for user input programmatically in console
## objectives
You will learn how to

+ wait for user input programmatically in console

## CH3.1 -- wait for user input programmatically in console
### `Console.ReadLine` static method
Only if the user inputs `Enter` on console, the code will continue to be executed 

and thus output print will continue to be executed.

### `Console.ReadKey` static method
Only if the user inputs any key on console, the code will continue to be executed 

and thus output print will continue to be executed.

Additionally, it will return `ConsoleKeyInfo` representing info of key that the user inputs.

> [!CAUTION]
> DON'T be confused with `Console.Read`.
> 
> `Console.Read` method call does NOT wait for user inputs.
>
> `Console.Read` is used to read one character for standard input stream.

## examples
### example 1
#### main code
Invoking following method

```
       /// <summary>
       /// illustrates how to stop executing code programmatically using `Console.ReadLine()` and `Console.ReadKey()` methods. 
       /// </summary>
       public static void TestMethod1()
       {
           Console.WriteLine("In {0} method call,", MethodBase.GetCurrentMethod().Name);

           Console.WriteLine("Pressing an `Enter` key will continue to execute the code, that is stopped by `Console.ReadLine()`.");
           Console.ReadLine();

           Console.WriteLine("Pressing any key will continue to execute the code, that is stopped by `Console.ReadKey()`.");
           Console.ReadKey();

           Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
       }
```

will looks like this:

[demo video](./stop%20executing%20code%20programmatically%20in%20CSharp.mkv)

## reference
### API docs
+ [`Console.ReadLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.readline?view=net-9.0)

+ [`Console.ReadKey Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.readkey?view=net-9.0)
