# CH10 -- detect key is available in console
## objectives
You will learn how to

+ detect key is available in console

## CH10.1 -- detect key is available in console
### `Console.KeyAvailable` getter-property
return true iff whether a key press is available in the input stream.

## examples
### example 1
In this example, it illustrates how to detect the key press in console is available.

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + detect the key press in console is available 
        /// </summary>
        public static void TestMethod13()
        {
            Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

            ConsoleKeyInfo key;

            do
            {
                Console.WriteLine("\nPress a key to display; press the 'x' key to quit.");

                // Your code could perform some useful task in the following loop. However,
                // for the sake of this example we'll merely pause for a quarter second.

                while(!Console.KeyAvailable)
                {
                    Thread.Sleep(250); // Loop until input is entered.
                }

                key = Console.ReadKey(true);
                Console.WriteLine("You pressed the '{0}' key." , key.Key);
            } while(key.Key != ConsoleKey.X);

            Console.WriteLine("GoodBye");

            Console.WriteLine("End of {0} method call",MethodBase.GetCurrentMethod().Name);
        }
```

## reference
### API docs
+ [`Console.KeyAvailable Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.keyavailable?view=net-8.0)

+ [`ConsoleKeyInfo Struct`](https://learn.microsoft.com/en-us/dotnet/api/system.consolekeyinfo?view=net-8.0)