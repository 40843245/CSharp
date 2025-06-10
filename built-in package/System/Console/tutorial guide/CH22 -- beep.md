# CH22 -- beep
## objectives
You will learn how to

+ beep

## CH22.1 -- beep
### `Console.Beep` static method

```
Console.Beep(440,500);
```

will beep with 440 Hz and duration 550 ms.

#### Remarks
1. The frequency MUST be set between 37 and 32767 Hz (both inclusive).

2. The duration MUST be positive integer.

Otherwise, it will throw [`ArgumentOutOfRangeException`](https://learn.microsoft.com/en-us/dotnet/api/system.console.beep?view=net-8.0) at runtime.

## examples
### example 1
#### main code
The following example illustrate how to beep with specific frequency and duration.

+ Use Case 1:

If the user type `S`, then one can customize the frequency and duration.

It will prompt user to type `frequency` and `duration`.

In this case, 

if the user types and invalid `frequency` (for available range of frequency, see above [Remarks](#remarks)), it will use default frequency. 

similarly, if the user types and invalid `duration` (for available range of duration, see above [Remarks](#remarks)), it will use default duration.

Finnally, it will beep by invoking `Console.Beep(frequency,duration);` 

+ Use case 2:

If the user does NOT type `S` or `X`, it will beep by invoking `Console.Beep();`

+ Use case 3:

If the user types `X`, it will exit the loop, terminating the code execution.

Invoking following method

```
        /// <summary>
        /// illustrates how to
        /// 
        /// + play a beep sound in the console application using `Console.Beep` method.
        /// </summary>
        public static void TestMethod21()
        {
            Console.ForegroundColor = ConsoleColor.White;

            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            ConsoleKeyInfo currentKeyInfo;
            int frequency = 800;
            int duration = 1000;
            Console.TreatControlCAsInput = true; // Treat `CTL+C` as input instead of terminating the application. 

            do
            {
                Console.WriteLine("Press any key to continue and then beep,\nor Press `S` to set the frequency and duration of beep then beep,\nor Press `X` key to exit.");
                KeyboardHelper.WaitUntilKeyAvailable(); // Wait until a key is available.

                currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.

                if(currentKeyInfo.Key == ConsoleKey.S)
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.WriteLine("You pressed `{0}`.Continuing..." , currentKeyInfo.GetInfo());

                    Console.WriteLine("Enter the frequency (in Hz) for the beep sound:");

                    //Console.Clear();

                    Console.ForegroundColor = ConsoleColor.Green;

                    string frequencyConsoleReadLine = Console.ReadLine();
                    Console.WriteLine("frequencyConsoleReadLine:{0}", frequencyConsoleReadLine);
                    if(!(int.TryParse(frequencyConsoleReadLine , out frequency) && frequency > 0))
                    {
                        frequency = Math.Clamp(
                            BeepConstants.DEFAULT_FREQUENCY ,
                            BeepConstants.MIN_FREQUENCY ,
                            BeepConstants.MAX_FREQUENCY
                        ); // Ensure duration does not exceed maximum allowed duration.
                        Console.WriteLine("Invalid frequency. Using default frequency of {0} Hz." , BeepConstants.DEFAULT_FREQUENCY);
                    }
                    else
                    {
                        frequency = Math.Clamp(
                            frequency ,
                            BeepConstants.MIN_FREQUENCY ,
                            BeepConstants.MAX_FREQUENCY
                        ); // Ensure duration does not exceed maximum allowed duration.
                        Console.WriteLine("Frequency set to {0} Hz." , frequency);
                    }

                    //Console.Clear();
                    Console.ReadLine();

                    string durationConsoleReadLine =  Console.ReadLine();
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.WriteLine("durationConsoleReadLine:{0}" , durationConsoleReadLine);
                    if(!(int.TryParse(durationConsoleReadLine , out duration) && duration > 0))
                    {
                        duration = Math.Max(1 , BeepConstants.DEFAULT_DURATION); // Ensure duration is at least 1 milliseconds.
                        Console.WriteLine("Invalid duration. Using default duration of {0} milliseconds." , BeepConstants.DEFAULT_DURATION);
                    }else
                    {                       
                        Console.WriteLine("Duration set to {0} milliseconds." , duration);
                    }

                    Console.ResetColor();
                    Console.WriteLine("Beeping with frequency {0} Hz for {1} milliseconds." , frequency , duration);
                    Console.Beep(frequency , duration);
                }
                else if(currentKeyInfo.Key == ConsoleKey.X)
                {
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    Console.WriteLine("You pressed `{0}`.Exiting..." , currentKeyInfo.GetInfo());
                    Console.ResetColor();
                    break;
                }
                else
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.WriteLine("You pressed `{0}`.Continuing..." , currentKeyInfo.GetInfo());
                    Console.ResetColor();
                    Console.WriteLine("Beeping...");
                    Console.Beep(); // Play a beep sound to indicate a key press.
                }
                Thread.Sleep(1000); // Prevent busy-waiting                
            } while(true);
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

might be shown as [demo video](https://github.com/40843245/CSharp/blob/master/demo%20of%20beep%20programmatically.mkv)

## reference
### API docs
+ [`Console.Beep Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.beep?view=net-8.0)
