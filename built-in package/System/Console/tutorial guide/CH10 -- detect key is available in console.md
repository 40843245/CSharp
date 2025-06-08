# CH10 -- detect key is available in console
## objectives
You will learn how to

+ detect key is available in console

## CH10.1 -- detect key is available in console
### `Console.KeyAvailable` getter-property
return true iff whether a key press is available in the input stream.

## examples
### example 1
In this example, the loop terminates iff when the user presses the "Clear" key.

```
        /// <summary>
        /// Executes a loop that prompts the user to press any key to continue or exit the loop.
        /// </summary>
        /// <remarks>This method continuously displays a message in the console, waiting for user input.
        /// The loop terminates when the user presses the "Clear" key.
        /// </remarks>
        public static void TestMethod9()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            while(true)
            {
                Console.WriteLine("Press any key to continue, or `cancel` to exit.");
                ConsoleKeyInfo key = Console.ReadKey(true); // read a key without displaying it in the console.
                if(key.Key == ConsoleKey.Clear)
                {
                    break;
                }

            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

## reference
### API docs
+ [`Console.KeyAvailable Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.keyavailable?view=net-8.0)

+ [`ConsoleKeyInfo Struct`](https://learn.microsoft.com/en-us/dotnet/api/system.consolekeyinfo?view=net-8.0)