# CH7 -- get position of cursor and move cursor
## objectives
You will learn how to

+ get position of cursor

+ move cursor

## CH7.1 -- get position of cursor
### `Console.GetCursorPosition` static method

```
ValueTuple<int,int> cursorPosition = Console.GetCursorPosition();
```

will return `ValueTuple<int,int>` (a tuple with elements named `Left`, `Top` respectively) representing current cursor position.

So, through unbox techinque, 

we can get 

+ left of cursor position (stored in variable `cursorLeftPosition2`)

+ top of cursor position (stored in variable `cursorTopPosition2`)

```
int cursorLeftPosition2, cursorTopPosition2;
// use unbox techique.
(cursorLeftPosition2, cursorTopPosition2) = Console.GetCursorPosition();
```

Meanwhile, we can get 

+ left of cursor position through accessing the element named `Left`

+ top of cursor position through accessing the element named `Top`

```
int cursorLeftPosition3, cursorTopPosition3;
cursorLeftPosition3 = Console.GetCursorPosition().Left;
cursorTopPosition3 = Console.GetCursorPosition().Top; 
```

### `Console.CursorLeft` getter-setter property and `Console.CursorTop` getter-setter property

On top of that, we can also get 

+ left of cursor position through accessing `Console.CursorLeft` getter-setter property

+ top of cursor position through accessing `Console.CursorTop` getter-setter property

```
int cursorLeftPosition1, cursorTopPosition1;
cursorLeftPosition1 = Console.CursorLeft;
cursorTopPosition1 = Console.CursorTop;
```

## CH7.2 -- move cursor
### `Console.SetCursorPosition` static method

```
Console.SetCursorPosition(10, 10); // move the cursor to position (10, 10).
```

will move the current cursor to (10,10).

### `Console.CursorLeft` getter-setter property and `Console.CursorTop` getter-setter property

On top of that, we can also set 

+ left of cursor position through setting `Console.CursorLeft` getter-setter property

+ top of cursor position through setting `Console.CursorTop` getter-setter property

```
Console.CursorLeft = 0; // move the left position cursor to 0
Console.CursorTop = 0; // move the left position cursor to 0
```

will move the current cursor to (0,0).

## examples 
### example 1
#### helper class

`Cursor.cs`

```
public static class Cursor
{
    public static void PrintCursorPosition()
    {
        int cursorLeftPosition1, cursorTopPosition1;
        int cursorLeftPosition2, cursorTopPosition2;
        int cursorLeftPosition3, cursorTopPosition3;

        cursorLeftPosition1 = Console.CursorLeft; // get the current cursor left position.
        cursorTopPosition1 = Console.CursorTop; // get the current cursor top position.

        // use unbox techique.
        (cursorLeftPosition2, cursorTopPosition2) = Console.GetCursorPosition(); // get current cursor position as a tuple.

        cursorLeftPosition3 = Console.GetCursorPosition().Left; // get current cursor position as a tuple and then access the zero element named `Left` to get the left position.
        cursorTopPosition3 = Console.GetCursorPosition().Top; // get current cursor position as a tuple and then access first element named `Top` to get the top position.

        Console.WriteLine("Current cursor position: ({0}, {1})" , cursorLeftPosition1 , cursorTopPosition1);

        Console.WriteLine("Current cursor position: ({0}, {1})" , cursorLeftPosition2 , cursorTopPosition2);

        Console.WriteLine("Current cursor position: ({0}, {1})" , cursorLeftPosition3 , cursorTopPosition3);
    }
}
```

#### main code
executing the following code

`Program.cs`

```
Console.WriteLine("---------------------------------------");
DemoClass1.TestMethod5();
Console.WriteLine("---------------------------------------");
Console.ReadLine();
```

`DemoClass1.cs`

```
    public static class DemoClass1
    {
        // ... omitted for clarity

        /// <summary>
        /// illustrates how to get and set the cursor position in the console window.
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.WriteLine("Ready to move cursor to position (10, 10)");

            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.SetCursorPosition(10, 10); // move the cursor to position (10, 10).

            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.WriteLine("After moving cursor to position (10, 10)");

            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.WriteLine("Ready to move cursor to position (0, 0)");

            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.CursorLeft = 0; // move the left position cursor to 0
            Console.CursorTop = 0; // move the left position cursor to 0
            
            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.WriteLine("After moving cursor to position (0, 0)");

            Cursor.PrintCursorPosition(); // print current cursor position.

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
    }
```

will output following

![result of TestMethod5](result%20of%20TestMethod5.png)

## reference
### API docs
+ [`Console.CursorLeft Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.cursorleft?view=net-9.0)

+ [`Console.CursorTop Property`](https://learn.microsoft.com/en-us/dotnet/api/system.console.cursortop?view=net-9.0)

+ [`Console.GetCursorPosition Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.getcursorposition?view=net-9.0)

+ [`Console.SetCursorPosition(Int32, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.setcursorposition?view=net-9.0)
