# CH11 -- change the title of console
## objectives
You will learn how to

+ change the title of console

## CH11.1 -- change the title of console
### `Console.Title` getter-setter property

```
string title = Console.Title;
```

will return the title of `Console`.

```
Console.Title = "The title has changed!";
```

the console of title will be changed to `The title has changed!`.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + change the console title
        /// </summary>
        public static void TestMethod11()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("The current console title is: \"{0}\"" ,
                      Console.Title);
            Console.WriteLine("  (Press any key to change the console title.)");
            Console.ReadKey(true);
            Console.Title = "The title has changed!";
            Console.WriteLine("Note that the new console title is \"{0}\"\n" +
                              "  (Press any key to quit.)" , Console.Title);
            Console.ReadKey(true);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

Executing by clicking playing button might output following

```
In TestMethod11 method call,
The current console title is: "D:\workspace\tutorial projects\built-in package\System\Console\Example\Example\bin\Debug\net8.0\Example.exe"
  (Press any key to change the console title.)
Note that the new console title is "The title has changed!"
  (Press any key to quit.)
End of TestMethod11 method call,
```

Executing `Example.exe` in `cmd.exe` will change the title.

![example of changing title of console.png](example%20of%20changing%20title%20of%20console.png)