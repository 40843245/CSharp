# Advanced Reading CH1 -- detect the key press state is changed after a key press
## objectives
You will  

+ see an example that detects the key press state is changed after a key press
+ its logic in the exmaple

## examples
### example 1
#### purpose
After the user enter a key (which holds one char) then type `enter` tab, 

the initial state of key press will be set with given the user input and the previous state is set to initial state.

then it will enter the infinite loop> 

In the loop

Step 1. it will prompt the user enter a key then type `enter` tab.

Step 2. the user will press an key and `enter` tab.

Step 3. it will return the info of key

> [!TIP]
> `Console.ReadKey` static method always returns a `ConsoleKeyInfo` instance (non-null) which
>
> stores the info of key in the first element of buffer (with data structure `queue`).

> [!TIP]
> the info of key can be stored into an `ConsoleKeyInfo` instance.

Step 4. then it will compare current state of key press and previous state.

    + Case 1:
   
      If their state are NOT considered to be equal,

      then it will print

      ```
      Your currently pressed key `{0}` is NOT same as previous one." , currentKeyInfo.KeyChar
      ```

      on console with red text.

      > [!NOTE]
      > where
      >
      > `currentKeyInfo` is stores the info of key the user inputs in this execution of loop.
      
      Then execute next step.

    + Case 2:

      Otherwise, execute next step.

Step 5. then it will check the user enters `X`.

    + Case 1:

      If the user enter `X`, it will print

      ```
      You pressed `{0}`.Exiting... , currentKeyInfo.GetInfo()
      ```

      on the console with yellow text
       
      > [!NOTE]
      > where
      >
      > `currentKeyInfo.GetInfo()` will return the info of the key that the user inputs in this execution of loop. Including
      >
      > + all key modifiers that user pressed.
      > + the key that user pressed.

      and then exit the loop.

    + Case 2:

      Otherwise, it will print

      ```
      You pressed `{0}`.Continuing... , currentKeyInfo.GetInfo()
      ```

      on the console with green text

      next, it will set the previous state of key press as current state

      and then continue the loop.

#### Review
Let's quick review some methods, properties and some concepts that will be used here.

If you forgot them, you can search their API in MSDS or see previous CH in my notes.

##### `Console.ForegroundColor` getter-setter property
To set the text that will be printed on console, set `Console.ForegroundColor` getter-setter property.

##### `Console.ResetColor` static method

```
Console.ResetColor();
```

will reset color to default color.

##### `Console.KeyAvailable` getter property
It will return `true` iff the user can press key on console, the key that user inputs might not display on the console.

##### clear the buffer
To stimulate to clear the buffer, there are two ways.

When the user can press key on console, consume all keys in buffer.

```
while(Console.KeyAvailable)
{
    Console.ReadKey(true); // Read and discard the key
}
```

or

```
while(Console.KeyAvailable)
{
    Console.ReadLine(); // Read and discard the key
}
```

##### wait until a key can be pressed by user before continuing execution of code

```
public static void WaitUntilKeyAvailable(int milliSeconds = 250)
{
    while(!Console.KeyAvailable)
    {
        // Wait until a key is pressed
        Thread.Sleep(milliSeconds); // Sleep to avoid busy waiting
    }
}
```

#### construct the code
After quick review, let's start to write the code.

##### Part 1 -- get the info of key
To return the info of the key. Including

+ all key modifiers that user pressed.

+ the key that user pressed.

For example, 

Use Case 1:

When the user presses `CTRL`+`SHIFT`+`B`  at same time with `NumberLock` is on, `CapsLock` is on and then presses `enter` tab.

We want to get `CapsLock+NumberLock+Control+Shift+B`.

Use Case 2:

When the user presses `CTRL`+`SHIFT`+`B`  at same time with `NumberLock` is on, `CapsLock` is off and then presses `enter` tab.

We want to get `NumberLock+Control+Shift+B`.

Use Case 3:

When the user presses `CTRL`+`SHIFT`+`ALT`+`B`  at same time with `NumberLock` is on, `CapsLock` is off and then presses `enter` tab.

We want to get `NumberLock+Alt+Control+Shift+B`.

Use Case 4:

When the user presses `B` at same time with `NumberLock` is on, `CapsLock` is off and then presses `enter` tab.

We want to get `NumberLock+B`.

Use Case 5:

When the user presses nothing at same time with `NumberLock` is on, `CapsLock` is off and then presses `enter` tab.

We want to get `NumberLock+<enter>` 

where 

`<enter>` is the key char of `enter` tab.

To do that, we can NOT simply convert an `ConsoleKeyInfo` instance to `string` with `ToString()` method call.

We have to write it by ourself.

For readability and maintenance, the author decides to define an extension method `ConsoleKeyInfo.GetInfo()` extension method as follows.

```
public static partial class ConsoleKeyInfoExtensionMethods
{
    public static string GetInfo(
        this ConsoleKeyInfo keyInfo
    )
    {
        // logic here
        // return a string
    }
}
```

1. First, the author will define a list (a `List<string>` instance) that stores `CapsLock`, `NumberLock`, all key modifers that user pressed, the user pressed key (for non-modifier), respectively.

```
List<string> textList = new List<string>();
```

2. We will detect the `CapsLock` is on.

```
if(Console.CapsLock)
{
    textList.Add("CapsLock");
}
```

3. We will detect the `NumberLock` is on.

```
if(Console.NumberLock)
{
    textList.Add("NumberLock");
}
```

4. Then we will use `switch` block to stores all key modifers that user inputs.

Since we want to print key modifier as following order (we can know it in Use Case 3 above)

    + `Control`
    + `Alt`
    + `Shift  

```
switch(keyInfo.Modifiers)
{
        case ConsoleModifiers.Control:
            textList.Add("Control");
            goto case ConsoleModifiers.Alt;
        case ConsoleModifiers.Alt:
            textList.Add("Alt");
            goto case ConsoleModifiers.Shift;
        case ConsoleModifiers.Shift:
            textList.Add("Shift");
            break;
        default:
            break;
}
```

Don't forget to use `goto case` to jump into specific case.

> [!IMPORTANT]
> In `C#`, a `case` block must have one of following as the last statement:
>
> + `break` statement
> + `goto` statement, `goto case` statement, or `goto default` statement
> + `return` statement
>
> Otherwise, it will throw an compiler error.
>
> To jump into next `case` block (performing statements in next `case` block),
>
> use `goto case` statement with given the label of next `case` block, or `goto default` statment.
>
> Its syntax is different than `C/C++`,
>
> In `C/C++`, to jump into `next` case block,
>
> simply don't add `break` statement as last statement of this `case` block.

5. To prevent the following ocurr:

When the user press `\0`,  it is added to indicate the string is end.

> [!NOTE]
> In `C/C++` and `C#`, the `\0` means the end of char or string.

We have to check the key char is `\0`.

If it is NOT `\0`, we will get it and store it into the list.

```
if(keyInfo.KeyChar != '\0')
{
    textList.Add(keyInfo.KeyChar.ToString());
}
```

6. Finally, we join the list into a string with separator `+`.

```
string.Join("+",textList)
```

7. Combining them, we have finish `ConsoleKeyInfo.GetInfo` extension method.

`ConsoleKeyInfoExtensionMethods.cs`

```
using System.Text;

namespace Example.Extensions.ExtensionMethods.ConsoleKeyInfoExtensionMethods
{
    public static partial class ConsoleKeyInfoExtensionMethods
    {
        public static string GetInfo(
            this ConsoleKeyInfo keyInfo
        )
        {
            List<string> textList = new List<string>();
            StringBuilder stringBuilder = new StringBuilder();

            if(Console.CapsLock)
            {
                textList.Add("CapsLock");
            }
            if(Console.NumberLock)
            {
                textList.Add("NumberLock");
            }
            switch(keyInfo.Modifiers)
            {
                    case ConsoleModifiers.Control:
                        textList.Add("Control");
                        goto case ConsoleModifiers.Alt;
                    case ConsoleModifiers.Alt:
                        textList.Add("Alt");
                        goto case ConsoleModifiers.Shift;
                    case ConsoleModifiers.Shift:
                        textList.Add("Shift");
                        break;
                    default:
                        break;
            }
            if(keyInfo.KeyChar != '\0')
            {
                textList.Add(keyInfo.KeyChar.ToString());
            }

            stringBuilder.Append(string.Join("+",textList));
            return stringBuilder.ToString();
        }
    }
}
```

#### Part 2 -- comparison of key pressed state (`KeyToggleDetector` class implementation)
1. How to compare key pressed state with given two `ConsoleKeyInfo` instance?

Let's review the rule.

It will return `true` iff all following requirements are satisfied.

+ have same key modifiers
+ the states (on or off state) of `CapsLock` are same
+ the states (on or off state) of `NumberLock` are same
+ key chars are same

2. For convenience, I wrap the key pressed state and into a class `KeyToggleDetector` and define a method that compares key pressed state.
 
3. To get key modifier, we can simply access `Modifiers` getter property,

Thus, we can check they have same key modifiers.

```
bool hasSameModifiers = currentKeyInfo.Modifiers == previousKeyInfo.Modifiers;
```

4. To get state (on or off state) of `CapsLock`, we can simply access `Console.CapsLock` static getter property.

Similary, To get state (on or off state) of `NumberLock`, we can simply access `Console.NumberLock` static getter property.

Thanks to the comparison `Tuple<TDelegate,...>`. 

> [!NOTE]
> Comparing two `Tuple<TDelegate,...>` instance are non-referentially equal with `Equal` instance method or `==` operator (the prequisite is that it is NOT overriden) will return true iff
>
> All following requirements are satisfied.
> 
> + They have same number of elements
> + In each corresponding element, they are  non-referentially equal.
>
> For more details, see [`Tuple equality`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples#tuple-equality)

I can wrap `Console.CapsLock` and `Console.NumberLock` into a `Tuple<bool,bool>` and compare them are non-referential equal.

```
public static Tuple<bool , bool> GetLockKeys(
    this ConsoleKeyInfo keyInfo
)
{
    return new Tuple<bool , bool>(
        Console.CapsLock ,
        Console.NumberLock
    );
}
```

```
bool hasSameLockKeys = currentKeyInfo.GetLockKeys() == previousKeyLock;
```

where 

`previousKeyLock` is a getter-setter property with `Tuple<bool,bool>` type that 

stores the return value of `previousKeyInfo.GetLockKeys()` when updating (here, it means that `UpdatePreviousKeyInfo` method in `KeyToggleDetector` is called)

Here `UpdatePreviousKeyInfo` is defined as follows.

```
public void UpdatePreviousKeyInfo(
    ConsoleKeyInfo keyInfo
)
{
    previousKeyInfo = keyInfo;
    previousKeyLock = keyInfo.GetLockKeys();
}
```

> [!CAUTION]
> Watch out for `GetLockKeys` method call.
>
> Since it returns a tuple that stores static getter-property whose value is changed as time goes by (`Console.CapsLock` and `Console.NumberLock` static getter-property),
>
> we have to call `GetLockKeys()` once the `previousKeyInfo` is changed or reassigned etc.
>
> **NEVER** writes `currentKeyInfo.GetLockKeys() == previousKeyInfo.GetLockKeys()`.
>
> It will always return true theoretically.

5. To check key chars are same, we can simply access `KeyChar` of two `ConsoleKeyInfo` instance then compare them.

```
bool hasSameKeyChar = currentKeyInfo.KeyChar == previousKeyInfo.KeyChar;
```

6. By rule mentioned in 2th point,

we can combine them together as follows.

```
public bool IsKeyStateChanged(
    ConsoleKeyInfo currentKeyInfo
) 
{
    bool hasSameModifiers = currentKeyInfo.Modifiers == previousKeyInfo.Modifiers;
    bool hasSameLockKeys = currentKeyInfo.GetLockKeys() == previousKeyLock;
    bool hasSameKeyChar = currentKeyInfo.KeyChar == previousKeyInfo.KeyChar;

    return !(hasSameModifiers && hasSameLockKeys && hasSameKeyChar);
}
```

7. Combining the update method `UpdatePreviousKeyInfo` together. The `KeyToggleDetector` class is well-defined.

`KeyToggleDetector.cs`

```
using Example.Extensions.ExtensionMethods.ConsoleKeyInfoExtensionMethods;
using System;

namespace Example.Helper.Keyboard
{
    public class KeyToggleDetector
    {
        public ConsoleKeyInfo previousKeyInfo { get; set; }
        private Tuple<bool,bool> previousKeyLock{ get; set; }
        public KeyToggleDetector(
            ConsoleKeyInfo keyInfo
        )
        {
            this.UpdatePreviousKeyInfo(keyInfo);
        }

        public void UpdatePreviousKeyInfo(
            ConsoleKeyInfo keyInfo
        )
        {
            previousKeyInfo = keyInfo;
            previousKeyLock = keyInfo.GetLockKeys();
        }
        public bool IsKeyStateChanged(
            ConsoleKeyInfo currentKeyInfo
        ) 
        {
            bool hasSameModifiers = currentKeyInfo.Modifiers == previousKeyInfo.Modifiers;
            bool hasSameLockKeys = currentKeyInfo.GetLockKeys() == previousKeyLock;
            bool hasSameKeyChar = currentKeyInfo.KeyChar == previousKeyInfo.KeyChar;

            return !(hasSameModifiers && hasSameLockKeys && hasSameKeyChar);
        }
    }
}
```

##### Part 3 -- clean buffer in console
1. To detect user input in loop, we have to clear all buffer in console (especially, after `Console.ReadKey` or `Console.ReadLine` method call)

The reason why we have to do so is that

when user inputs a key and press `enter` tab to continue executing code, the user input and `enter` tab will be placed into buffer (place two chars into buffer).

However, `Console.ReadKey(true)` method call consume one char in buffer and return the char (with `ConsoleKeyInfo` type).

There are left one redundant `enter` tab, so we need to clear all buffer in console.

We can do this by 

```
while(Console.KeyAvailable)
{
    Console.ReadKey(true); // Read and discard the key
}
```

But for more readable and more maintenable, the author decides to wrap it into `ClearConsoleInputBuffer` static method in `KeyboardHelper` static class as follows.

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

    //... omitted for clarity
}
```

##### Part 4 -- main code
After finishing writing all utility class and helper class,

let's start to use it.

1. 

To set the initial state of key press with given the user input and the previous state is set to initial state 

after the user enter a key (which holds one char) then type `enter` tab,

we write these code

```
ConsoleKeyInfo previousKeyInfo;
ConsoleKeyInfo currentKeyInfo;

Console.TreatControlCAsInput = true; // Treat `CTL+C` as input instead of terminating the application.

Console.WriteLine("Press any key to continue.");
previousKeyInfo = Console.ReadKey(true);

Console.WriteLine("You pressed `{0}`." , previousKeyInfo.GetInfo());
KeyToggleDetector keyToggleDetector;

do
{
  keyToggleDetector = new KeyToggleDetector(previousKeyInfo);
  //... omitted            
} while(true);
```

2. To complete Step 1. to Step 3. in purpose section, we write

```
do
{
  //... omitted
  Console.WriteLine("Press any key to continue or Press `X` key to exit.");
  KeyboardHelper.ClearConsoleInputBuffer(); // Clear any pending input from the console's input buffer.
  currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.
  //... omitted
} while(true);
```

3. To complete Step 5 (comparing two key press states), we just invoke `IsKeyStateChanged` of `KeyToggleDetector` instance as follows.

```
do
{
  //... omitted
  currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.
  //... omitted
  if(keyToggleDetector.IsKeyStateChanged(currentKeyInfo))
  {
    //... logic here
  }
  //... omitted
} while(true);
```

4. To complete the case 2 in Step 5 (if two key press states is considered to be equal, print message on console with green text), we write

```
do
{
  //... omitted
  currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.
  //... omitted
  if(keyToggleDetector.IsKeyStateChanged(currentKeyInfo))
  {
    Console.ForegroundColor = ConsoleColor.Red;
    Console.WriteLine("Your currently pressed key `{0}` is NOT same as previous one." , currentKeyInfo.KeyChar);
    Console.ResetColor();
  }
  //... omitted
} while(true);
```

5. To complete Step 6 (detect the user presses `X` at current), we write

```
do
{
  //... omitted
  if(currentKeyInfo.Key == ConsoleKey.X)
  {
    //... logic here.
  }
  //... omitted
}while(true);
```

6. To complete case 1 of Step 6 (if the user presses `X` at current, print message on console with yello text then exiting the loop), we write

```
do
{
  //... omitted
  if(currentKeyInfo.Key == ConsoleKey.X)
  {
    Console.ForegroundColor = ConsoleColor.Yellow;
    Console.WriteLine("You pressed `{0}`.Exiting..." , currentKeyInfo.GetInfo());
    Console.ResetColor();
    break;
  }
  //... omitted
}while(true);
```

7. Don't forget to update the key info.

```
previousKeyInfo = currentKeyInfo;
```

8. To prevent busy-waiting, the thread needs to sleep for 1000 ms.
   
```
Thread.Sleep(1000); // Prevent busy-waiting    
```

9. Combining them together, we are done.

```
        /// <summary>
        /// illustrates how to
        /// 
        /// + detect if the key state has changed since the last key press
        /// </summary>
        public static void TestMethod20()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ConsoleKeyInfo previousKeyInfo;
            ConsoleKeyInfo currentKeyInfo;

            Console.TreatControlCAsInput = true; // Treat `CTL+C` as input instead of terminating the application.

            Console.WriteLine("Press any key to continue.");
            previousKeyInfo = Console.ReadKey(true);

            Console.WriteLine("You pressed `{0}`." , previousKeyInfo.GetInfo());
            KeyToggleDetector keyToggleDetector;

            do
            {
                keyToggleDetector = new KeyToggleDetector(previousKeyInfo);

                Console.WriteLine("Press any key to continue or Press `X` key to exit.");
                KeyboardHelper.ClearConsoleInputBuffer(); // Clear any pending input from the console's input buffer.
                currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.
                if(keyToggleDetector.IsKeyStateChanged(currentKeyInfo))
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Your currently pressed key `{0}` is NOT same as previous one." , currentKeyInfo.KeyChar);
                    Console.ResetColor();
                }
                if(currentKeyInfo.Key == ConsoleKey.X)
                {
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    Console.WriteLine("You pressed `{0}`.Exiting..." , currentKeyInfo.GetInfo());
                    Console.ResetColor();
                    break;
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine("You pressed `{0}`.Continuing..." , currentKeyInfo.GetInfo());
                Console.ResetColor();

                previousKeyInfo = currentKeyInfo;
                Thread.Sleep(1000); // Prevent busy-waiting                
            } while(true);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```


#### code
##### utility class
see demo project

##### main code

```
        /// <summary>
        /// illustrates how to
        /// 
        /// + detect if the key state has changed since the last key press
        /// </summary>
        public static void TestMethod20()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ConsoleKeyInfo previousKeyInfo;
            ConsoleKeyInfo currentKeyInfo;

            Console.TreatControlCAsInput = true; // Treat `CTL+C` as input instead of terminating the application.

            Console.WriteLine("Press any key to continue.");
            previousKeyInfo = Console.ReadKey(true);

            Console.WriteLine("You pressed `{0}`." , previousKeyInfo.GetInfo());
            KeyToggleDetector keyToggleDetector;

            do
            {
                keyToggleDetector = new KeyToggleDetector(previousKeyInfo);

                Console.WriteLine("Press any key to continue or Press `X` key to exit.");
                KeyboardHelper.ClearConsoleInputBuffer(); // Clear any pending input from the console's input buffer.
                currentKeyInfo = Console.ReadKey(true); // read a key without displaying it in the console.
                if(keyToggleDetector.IsKeyStateChanged(currentKeyInfo))
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Your currently pressed key `{0}` is NOT same as previous one." , currentKeyInfo.KeyChar);
                    Console.ResetColor();
                }
                if(currentKeyInfo.Key == ConsoleKey.X)
                {
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    Console.WriteLine("You pressed `{0}`.Exiting..." , currentKeyInfo.GetInfo());
                    Console.ResetColor();
                    break;
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine("You pressed `{0}`.Continuing..." , currentKeyInfo.GetInfo());
                Console.ResetColor();

                previousKeyInfo = currentKeyInfo;
                Thread.Sleep(1000); // Prevent busy-waiting                
            } while(true);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

[Demo video of executing this method](https://github.com/40843245/CSharp/blob/master/demo%20of%20check%20the%20pressed%20key%20in%20console%20is%20NOT%20same%20as%20before.mkv)
