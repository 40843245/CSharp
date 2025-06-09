# CH8 -- change visibilty of cursor of console
## objectives
You will learn how to

+ change visibilty of cursor of console

+ get visibilty of cursor of console

## CH8.1 -- change visibilty of cursor of console
### `Console.CursorVisible` getter-setter property

```
Console.CursorVisible = true;
```

will change visibilty of cursor of console to true 

so that we can see cursor in window.

```
bool originalCursorVisibile = Console.CursorVisible;
```

will return the visibilty of cursor of console.

## CH8.2 -- get visibilty of cursor of console
see above section.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrates how to change the visibility of the cursor in the console window.
        /// </summary>
        public static void TestMethod6()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            
            string formattingString = "\nThe cursor is {0}.\nType any text then press Enter. " +
                "Type '+' in the first column to show \n" +
                "the cursor, '-' to hide the cursor, " +
                "or lowercase 'x' to quit:";
            string s;
            bool originalCursorVisibile;
            int originalCursorSize;

            Console.CursorVisible = true; // Initialize the cursor to visible.
            originalCursorVisibile = Console.CursorVisible;
            originalCursorSize = Console.CursorSize;
            Console.CursorSize = 100;     // Emphasize the cursor.

            while(true)
            {
                Console.WriteLine(
                    formattingString ,
                    Console.CursorVisible ? "VISIBLE" : "HIDDEN"
                );
                s = Console.ReadLine();
                if(!string.IsNullOrEmpty(s))
                {
                    if(s [ 0 ] == '+')
                    {
                        Console.CursorVisible = true;
                    }
                    else if(s [ 0 ] == '-')
                    {
                        Console.CursorVisible = false;
                    }
                    else if(s [ 0 ] == 'x')
                    {
                        break;
                    }
                }
            }
            Console.CursorVisible = originalCursorVisibile;
            Console.CursorSize = originalCursorSize;

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

may produce following output:

[demo video](demo%20of%20change%20visibility%20of%20cursor%20of%20window%20programmatically%20in%20CSharp.mkv)

## reference
### API docs
+ [`Console.CursorVisible Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.cursorvisible?view=net-9.0)

### examples
+ The code of example 1 is referenced from the first example in [`Console.CursorVisible Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.cursorvisible?view=net-9.0)