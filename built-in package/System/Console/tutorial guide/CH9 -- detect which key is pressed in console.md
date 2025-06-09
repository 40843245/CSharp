# CH9 -- detect which key is pressed in console
## objectives
You will learn how to

+ detect which key is pressed in console

## CH9.1 -- detect which key is pressed in console
### `ConsoleKeyInfo.Key` instance method
`Console.ReadKey` will return `ConsoleKeyInfo` instance.

The `KeyChar` getter-property of `ConsoleKeyInfo` instance 

will return a char that user inputs.

```
ConsoleKeyInfo key = Console.ReadKey(true);
Console.WriteLine("The user pressed the {0} key",key.KeyChar);
```

Additionally,

the `Key` getter-property of `ConsoleKeyInfo` instance 

will return an `ConsoleKey` enumeration value.

It is useful and more convenient to detect which key is pressed.

```
ConsoleKeyInfo key = Console.ReadKey(true);
if(key.Key == ConsoleKey.C)
{
    // if the user pressed the `C`.
}

if(key.Key == ConsoleKey.F1)
{
    // if the user pressed the `F1`.
}

if(key.Key == ConsoleKey.Clear)
{
    // if the user pressed the `clear` key.
    break;
}
```

### `ConsoleKeyInfo.Modifier` instance method
> [!IMPORTANT]
> Key modifiers in computer science are those keys on keyboard that 
> 
> can be detected whether they will be pressed with other key at same time.
>
> For example,
> 
> the OS can detect the user is pressed NOT ONLY `CTL` + `C`
>
> BUT ALSO `CTL` + `R` at same time.
>
> So, `CTL` (control key) is a key modifier.
>
> Key modifiers for Windows include:
>
> + `CTL`: control key
> + `Alt`: `Alt` key
> + `Shift`: `Shift` key.

In `C#`, the `ConsoleModifiers` enum value are consist of following key modifier.

| Name | Value | Description | 
| :-- | :-- | :-- |
| None | 0 | | 
| Alt | 1 | The left or right ALT modifier key. |
| Shift | 2 | The left or right SHIFT modifier key. |
| Control | 4 | The left or right CTRL modifier key. |

### `Console.CapsLock` static getter-property
`Console.CapsLock` static getter-property will return `true` 

iff the `CapLocks` in the keyboard (that the user is current used) is pressed.

### `Console.NumberLock` static getter-property
`Console.NumberLock` static getter-property will return `true` 

iff the `NumberLock` in the keyboard (that the user is current used) is pressed.

## examples
### example 1
#### main code
Invoking the following method

```
        /// <summary>
        /// illustrate how to 
        /// 
        /// + detect `CapsLock` is pressed
        /// 
        /// + detect `NumLock` is pressed
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("CapsLock is pressed:{0}" , Console.CapsLock);
            Console.WriteLine("NumLock is pressed:{0}" , Console.NumberLock);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

may produce following output

```
In TestMethod8 method call,
CapsLock is pressed:False
NumLock is pressed:True
End of TestMethod8 method call,
```

### example 2
#### main code
The following method will be exit iff the user inputs clear key.

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
+ [`Console.CapsLock Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.capslock?view=net-8.0)

+ [`Console.NumberLock Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.numberlock?view=net-8.0)

+ [`Console.ReadKey Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.readkey?view=net-8.0)

+ [`ConsoleKeyInfo Struct`](https://learn.microsoft.com/en-us/dotnet/api/system.consolekeyinfo?view=net-8.0)

+ [`ConsoleModifiers Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.consolemodifiers?view=net-9.0)

