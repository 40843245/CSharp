# CH1 -- detect cancel key is pressed
## objectives
You will learn how to

+ detect cancel key is pressed

# CH1.1 -- detect cancel key is pressed
### `Console.CancelKeyPress` event handler
When the cancel key (`CTL`+`Fn`+`B` key) is pressed in console,

events in `Console.CancelKeyPress` event handler will be triggered.

+ You can add an event (`ConsoleCancelEventHandler` delegate) to `Console.CancelKeyPress` event handler through `+=` operator

```
Console.CancelKeyPress += (sender, args) =>
{
    Console.WriteLine("Cancel key pressed. Exiting gracefully..."); // check the cancel key press is detect.

    // your code

    args.Cancel = true; // Prevents the process from terminating immediately

    // your code
};
```

or

```
CancelKeyPressEventHandler cancelKeyPressEventHandler = new CancelKeyPressEventHandler();
Console.CancelKeyPress += cancelKeyPressEventHandler.CancelKeyPressEvent;
```

where 

`cancelKeyPressEventHandler.CancelKeyPressEvent` is a `ConsoleCancelEventHandler` delegate

+ Similarly, you can remove an event (`ConsoleCancelEventHandler` delegate) to `Console.CancelKeyPress` event handler through `-=` operator

```
CancelKeyPressEventHandler cancelKeyPressEventHandler = new CancelKeyPressEventHandler();
Console.CancelKeyPress += cancelKeyPressEventHandler.CancelKeyPressEvent;
Console.CancelKeyPress -= cancelKeyPressEventHandler.CancelKeyPressEvent;
```

### `ConsoleCancelEventHandler` delegate
#### Definition
```
public delegate void ConsoleCancelEventHandler(
    object? sender, 
    ConsoleCancelEventArgs e
);
```

where

+ `sender` indicates the source of the event (who trigger this event).

+ `e` indicates event data.

### `ConsoleCancelEventArgs.Cancel` getter-setter property
#### definition
```
public bool Cancel { get; set; }
```

#### value
+ set it to `true` iff the current process should resume when the event handler concludes.

+ set it to `false` iff the current process should terminate when the event handler concludes.

## examples
### example 1
#### main code
The following method illustrates how to

+ detect cancel key is pressed through `Console.CancelKeyPress` event

+ prevent the process from terminating immediately 

when the `Console.CancelKeyPress` event handler (by setting `args.Cancel = true`)

During following method call,

```
        /// <summary>
        /// illustrates how to 
        /// 
        /// + detect cancel key is pressed through `Console.CancelKeyPress` event
        /// 
        /// + prevent the process from terminating immediately 
        /// 
        /// when the `Console.CancelKeyPress` event concludes (by setting `args.Cancel = true`)
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

            Console.TreatControlCAsInput = true; // This line is optional, it allows Control+C to be treated as input rather than terminating the process immediately

            Console.CancelKeyPress += (sender , args) =>
            {
                Console.WriteLine("Cancel key pressed. Exiting gracefully...");
                bool isControlCPressed, isControlBreakPressed;
                (isControlCPressed, isControlBreakPressed) = args.GetControlModifiers(); // This line is just to demonstrate the use of the extension method
                Console.WriteLine("Special Key: `{0}`" , args.SpecialKey);
                Console.WriteLine("Is Control+C Pressed?`{0}`" , isControlCPressed);
                Console.WriteLine("Is Control+Break Pressed?`{0}`" , isControlBreakPressed);
                Console.WriteLine("args.Cancel: `{0}`" , args.Cancel);
                Console.WriteLine("args.Equal(true): `{0}`" , args.Equals(true));
                args.Cancel = true; // Prevents the process from terminating immediately
                Console.WriteLine("args.Cancel: `{0}`" , args.Cancel);
                Console.WriteLine("args.Equal(true): `{0}`" , args.Equals(true));
            };

            

            Console.WriteLine("Press any key to continue or `Ctrl+Fn+B` to cancel...");
            Console.ReadKey(true); // Wait for a key press to continue
            
            Console.WriteLine("Press any key to exit the method...");
            Console.ReadKey(true); // Wait for a key press to continue
            Console.WriteLine("End of {0} method call,",MethodBase.GetCurrentMethod().Name);
        }
```

if the user enters `CTL`+`Windows`+`B` then enter cancel key (`CTL`+`Fn`+`B`),

it will produce the following output and continue to run the program without terminating.

![demo of detect cancel key press with args.Cancel = true.png](./demo%20of%20detect%20cancel%20key%20press%20with%20args.Cancel%20=%20true.png)

### example 2
#### main code
The following method illustrates how to

+ detect cancel key is pressed through `Console.CancelKeyPress` event

+ terminate the process immediately

when the `Console.CancelKeyPress` event concludes (by setting `args.Cancel = false`).

During following method call,

```
/// <summary>
/// illustrates how to 
/// 
/// + detect cancel key is pressed through `Console.CancelKeyPress` event
/// 
/// + terminate the process immediately
/// 
/// when the `Console.CancelKeyPress` event concludes (by setting `args.Cancel = false`).
/// </summary>
public static void TestMethod2()
{
    Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
    
    Console.TreatControlCAsInput = true; // This line is optional, it allows Control+C to be treated as input rather than terminating the process immediately

    Console.CancelKeyPress += (sender , args) =>
    {
        Console.WriteLine("Cancel key pressed. Exiting gracefully...");
        bool isControlCPressed, isControlBreakPressed;
        (isControlCPressed, isControlBreakPressed) = args.GetControlModifiers(); // This line is just to demonstrate the use of the extension method
        Console.WriteLine("Special Key: `{0}`" , args.SpecialKey);
        Console.WriteLine("Is Control+C Pressed?`{0}`" , isControlCPressed);
        Console.WriteLine("Is Control+Break Pressed?`{0}`" , isControlBreakPressed);
        Console.WriteLine("args.Cancel: `{0}`" , args.Cancel);
        Console.WriteLine("args.Equal(true): `{0}`" , args.Equals(true));
        args.Cancel = false; // terminates the process immediately
        Console.WriteLine("args.Cancel: `{0}`" , args.Cancel);
        Console.WriteLine("args.Equal(true): `{0}`" , args.Equals(true));
    };

    Console.WriteLine("Press any key to continue or `Ctrl+Fn+B` to cancel...");
    Console.ReadKey(true); // Wait for a key press to continue

    Console.WriteLine("Press any key to exit the method...");
    Console.ReadKey(true); // Wait for a key press to continue

    Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
}
```

if the user enters `CTL`+`Windows`+`B` then enter cancel key (`CTL`+`Fn`+`B`),

it will produce the following output and continue to run the program, but this process is got stuck.

![demo of detect cancel key press with args.Cancel = false.png](./demo%20of%20detect%20cancel%20key%20press%20with%20args.Cancel%20=%20false.png)

## reference
### API docs
+ [`Console.CancelKeyPress Event`](https://learn.microsoft.com/en-us/dotnet/api/system.console.cancelkeypress?view=net-9.0)

+ [`ConsoleCancelEventHandler Delegate`](https://learn.microsoft.com/en-us/dotnet/api/system.consolecanceleventhandler?view=net-9.0)

+ [`ConsoleCancelEventArgs Class`](https://learn.microsoft.com/en-us/dotnet/api/system.consolecanceleventargs?view=net-9.0)

+ [`ConsoleCancelEventArgs.Cancel Property`](https://learn.microsoft.com/en-us/dotnet/api/system.consolecanceleventargs.cancel?view=net-9.0)

