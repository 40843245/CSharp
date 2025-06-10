# CH23 -- handle cancel press event
## objectives
You will learn how to

+ handle cancel press event

## CH23.1 -- handle cancel press event
### `Console.CancelKeyPress` event handler
You can add or remove `ConsoleCancelEventHandler` event handler 

to handle cancel press event (i.e. when cancel key (`CTL+Fn+B` )is pressed)

You can add `ConsoleCancelEventHandler` event handler by `+=` operation.

A `ConsoleCancelEventHandler` event handler can be anyomous 

delagate or a delegate defined in a class.

Examples of adding `ConsoleCancelEventHandler` event handler by `+=` operation.

Example 1:

```
Console.CancelKeyPress += (sender , args) =>
{
    Console.WriteLine("Cancel key press event triggered. Exiting...");
    args.Cancel = true;
    Environment.Exit(0); // Exit the application gracefully
};
```

Example 2:

```
CancelKeyPressEventHandler cancelKeyPressEventHandler = new CancelKeyPressEventHandler();
Console.CancelKeyPress += cancelKeyPressEventHandler.CancelKeyPressEvent;
```

where

`CancelKeyPressEventHandler` class is defined as follows.

```
public class CancelKeyPressEventHandler
{
    public void CancelKeyPressEvent(object  sender , ConsoleCancelEventArgs args)
    {
        Console.WriteLine("CancelKeyPressEvent event triggered.");
        args.Cancel = true;
    }
}
```

### `System.ConsoleCancelEventHandler` delegate
#### Definition
```
public delegate void ConsoleCancelEventHandler(object? sender , ConsoleCancelEventArgs e);
```

where

`sender` indicates who triggers this event (and thus emit the singal).

`e` indicates the event data.

## examples
### example 1
#### main code
In the following example, when the user types `CTL`+ `Fn` + `B`, it will trigger `Console.CancelKeyPress` event handler.

Invoking following method

```
        /// <summary>
        /// illstrates how to 
        /// 
        /// + handle cancel key press event in the console application.
        /// </summary>
        public static void TestMethod19()
        {
            StreamWriter standardOutputStream = new StreamWriter(Console.OpenStandardOutput())
            {
                AutoFlush = true
            };
            Console.SetOut(standardOutputStream);

            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Console.TreatControlCAsInput = true; // Treat `CTL+C` as input instead of terminating the application.

            CancelKeyPressEventHandler cancelKeyPressEventHandler = new CancelKeyPressEventHandler();
            Console.CancelKeyPress += cancelKeyPressEventHandler.CancelKeyPressEvent;
            Console.CancelKeyPress += (sender , args) =>
            {
                Console.WriteLine("Cancel key press event triggered. Exiting...");
                args.Cancel = true;
                Environment.Exit(0); // Exit the application gracefully
            };

            ConsoleKeyInfo key;
            do
            {
                Console.WriteLine("Press any key to continue or Press `X` key to exit, or Press cancel key (i.e. `CTL`+`C` or `CTL`+ `Fn` + `B` key) to trigger the cancel key press event.\nNOTE that DNOT press `CTL`+ `Fn` + `F11`.\nOtherwise, an exception was thrown at runtime.");
                StreamReader standardInputStream = new StreamReader(Console.OpenStandardInput());
                Console.SetIn(standardInputStream);

                key = Console.ReadKey(true);
            } while(key.Key != ConsoleKey.X);
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output like this

![example of handling cancel press event.png](./example%20of%20handling%20cancel%20press%20event.png)

[demo video](https://github.com/40843245/CSharp/blob/master/demo%20of%20handle%20cancel%20press%20key%20event%20in%20console%20input%20programmatically%20in%20C%23.mkv)
