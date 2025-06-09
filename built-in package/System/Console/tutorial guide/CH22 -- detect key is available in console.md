# CH22 -- detect key is available in console
## objectives
You will learn how to

+ detect key is available in console

## CH22.1 -- detect key is available in console
### `Console.KeyAvailable` getter-property
returns `true` iff the key is available in console (i.e. the key pressed by user input can be handled).

## examples
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

## reference
### API docs
+ [`Console.KeyAvailable Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.keyavailable?view=net-8.0)