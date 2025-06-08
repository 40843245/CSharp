# CH12 -- how to prevent ending from pressing `CTL+C` in console
## objectives
You will learn how to

+ prevent ending from pressing `CTL+C` in console

## CH12.1 -- how to prevent ending from pressing `CTL+C` in console
### `Console.TreatControlCAsInput` getter-setter property

```
Console.TreatControlCAsInput = true;
```

will treat `CTL+C` as input in console to 

prevent ending from pressing `CTL+C` in console.

```
bool isControlCTreadAsInput Console.TreatControlCAsInput;
```

will get treat `CTL+C` as input in console.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + prevent example from ending if `CTL+C` is pressed
        /// </summary>
        public static void TestMethod12()
        {
            ConsoleKeyInfo key;
            // Prevent example from ending if CTL+C is pressed.
            Console.TreatControlCAsInput = true;

            Console.WriteLine("Press any combination of CTL, ALT, and SHIFT, and a console key.");
            Console.WriteLine("Press the Escape (Esc) key to quit: \n");
            do
            {
                key = Console.ReadKey();
                Console.Write(" --- You pressed ");
                if((key.Modifiers & ConsoleModifiers.Alt) != 0)
                {
                    Console.Write("ALT+");
                }
                if((key.Modifiers & ConsoleModifiers.Shift) != 0)
                {
                    Console.Write("SHIFT+");
                }
                if((key.Modifiers & ConsoleModifiers.Control) != 0)
                {
                    Console.Write("CTL+");
                }
                Console.WriteLine(key.KeyChar);
            } while(key.Key != ConsoleKey.Escape);
        
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

might output following

[demo project](demo%20of%20pressing%20control%20in%20console.mkv)

## reference
### API docs
+ [`Console.TreatControlCAsInput Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.treatcontrolcasinput?view=net-8.0)

+ [`ConsoleKeyInfo.Modifiers Property`](https://learn.microsoft.com/en-us/dotnet/api/system.consolekeyinfo.modifiers?view=net-8.0)

+ [`ConsoleModifiers Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.consolemodifiers?view=net-9.0)