# CH10 -- detect key is available in console
## objectives
You will learn how to

+ detect key is available in console

## CH10.1 -- detect key is available in console
### `Console.KeyAvailable` getter-property
return true iff whether a key press is available in the input stream.

## examples
### example 1
### example 1
#### main code
The following method will simulate how to clear the buffer -- a queue that 

stores the key from user inputs 

(i.e. return value of `Console.ReadKey` static method call).

The user inputs during the code is sleeping,

will be put in the queue and immediately be consumed, so that

after sleeping, the queue does NOT contain the user inputs during the code is sleeping.

This can simulate the simulate how to clear the buffer

```
public static class KeyboardHelper
{
    /// <summary>
    /// Clears any pending input from the console's input buffer.
    /// </summary>
    public static void ClearConsoleInputBuffer()
    {
        while(Console.KeyAvailable)
        {
            Console.ReadKey(true); // Read and discard the key
        }
    }
}
```

And it can be used as follows.

```
do
{
    Console.WriteLine("Press any key to continue or Press `X` key to exit.");
    KeyboardHelper.ClearConsoleInputBuffer(); // Clear any pending input from the console's input buffer.
    currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.
    Console.ForegroundColor = ConsoleColor.Green;
    Console.WriteLine("You pressed `{0}`.Continuing..." , currentKeyInfo.GetInfo());
    Console.ResetColor();

    previousKeyInfo = currentKeyInfo;
    Thread.Sleep(1000); // Prevent busy-waiting                
} while(true);
```

### example 2
#### main code
The following method, it illustrates a better and flexible way to do so in example 1.

```
       /// <summary>
       /// waits until a key is pressed before continuing execution.
       /// </summary>
       public static void WaitUntilKeyAvailable()
       {
           while(!Console.KeyAvailable)
           {
               // Wait until a key is pressed
               Thread.Sleep(250); // Sleep to avoid busy waiting
           }
       }
```

And it can be used as follows.

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