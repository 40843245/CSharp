# CH6 -- resize window
You will learn how to

+ resize window

## CH6.1 -- resize window
### Remarks
Came up with [Google Gemini's response](https://g.co/gemini/share/12b161ada1f6),

I've followed the instruction and I've tried in other case.

I found these facts.

1. In different command line prompt, 

there are different way to resize window.

For example,

Assuming there is a script containing the code snippet.

`Program.cs`

```
Console.WindowHeight = 2; // set height of window as 2 rows.
```

After compiling, it will generate `Example.exe`.

+ After opening `cmd` with executing `cmd.exe` and execute the above script.

It will resize height the window (created by executing `cmd.exe`) 

to 2 rows height.

![example of resize height of window as 2 row programmtically that code is executed by `cmd.exe`](example%20of%20resize%20height%20of%20window%20as%202%20rows.png)

+ When I click `Example.exe` to execute it, 

it will pop up a command line prompt, 

but it will NOT resize the window (created by executing `Example.exe` through directly click it). 

![example of NOT resize height of window as 2 rows by click Example.exe](example%20of%20NOT%20resize%20height%20of%20window%20as%202%20rows%20by%20click%20Example.exe.png)

+ When I execute the project by clicking the playing button in VS,

it will also NOT resize the window (created by executing `Example.exe` in `VS Terminal`).

![example of NOT resize height of window as 2 rows by executing project in VS](example%20of%20NOT%20resize%20height%20of%20window%20as%202%20rows%20by%20executing%20project%20in%20VS.png)

Thanks to [Google Gemini's response](https://g.co/gemini/share/12b161ada1f6) which gives an detail explanation and the author came up with its response.

### `Console.WindowHeight` property

```
int height = Console.WindowHeight;
```

will return height of current window 

```
Console.WindowHeight = 2; // set height of window as 2 rows height.
```

will resize the height of window as 2 rows height.

### `Console.WindowWidth` property

```
int width = Console.WindowHeight;
```

will return the width of current window

```
Console.WindowHeight = 2; // set height of window as 2 rows height.
```

will resize the width of window as 2 columns width.

### `Console.WindowLeft` property

```
int leftPosition = Console.WindowLeft;
```

will return the left position of current window relative to intial position of current window (if possible).

```
Console.WindowLeft = 20; // set x coordinate of window as 20 relative to the position of initial window
```

will set the left position of current window as 20 relative to intial position of current window (if possible). 

Otherwise, throw an exception at runtime.

### `Console.WindowTop` property

```
int topPosition = Console.WindowTop;
```

will return the top position of current window relative to intial position of current window (if possible).

```
Console.WindowTop = 20; // set y coordinate of window as 20 relative to the position of initial window
```

will set the top position of current window as 20 relative to intial position of current window (if possible). 

Otherwise, throw an exception at runtime.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrates how to 
        /// 
        /// + get and change the console window size
        /// 
        /// + get and change the console window position
        /// 
        /// + get buffer area size
        /// </summary>
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("current window height:{0}",Console.WindowHeight);

            Console.WindowHeight = 40; // set height of window as 40 rows.

            Console.WriteLine("current window height:{0}" , Console.WindowHeight);

            Console.WriteLine("current window width:{0}" , Console.WindowWidth);

            Console.WindowWidth = 50; // set width of window as 50 columns.

            Console.WriteLine("current window width:{0}" , Console.WindowWidth);

            Console.WriteLine("width of buffer area:{0}", Console.BufferWidth);

            Console.WriteLine("current window left position:{0}" , Console.WindowLeft);

            // Calculate the maximum allowed WindowLeft
            int maxAllowedWindowLeft = Console.BufferWidth - Console.WindowWidth;

            Console.WindowLeft = Math.Max(0, Math.Min(20 , maxAllowedWindowLeft)); // set x coordinate of window as 20 relative to the initial position of window (if possible).

            Console.WriteLine("current window left position:{0}" , Console.WindowLeft);

            Console.WriteLine("Height of buffer area:{0}" , Console.BufferHeight);

            Console.WriteLine("current window top position:{0}" , Console.WindowLeft);

            // Calculate the maximum allowed WindowTop
            int maxAllowedWindowTop = Console.BufferHeight - Console.WindowHeight;

            Console.WindowTop = Math.Max(0 , Math.Min(20 , maxAllowedWindowTop)); // set y coordinate of window as 20 relative to the initial position of window (if possible).

            Console.WriteLine("current window top position:{0}" , Console.WindowTop);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

+ Executing above snippets by click playing button in VS will output following, but NOT resize the window.

```
In TestMethod4 method call,
current window height:30
current window height:40
current window width:120
current window width:50
width of buffer area:50
current window left position:0
current window left position:0
Height of buffer area:40
current window top position:0
current window top position:0
End of TestMethod4 method call,
```

+ Executing it by executing `Example.exe` in `cmd.exe` will resize the window as follows.

![result of TestMethod4.png](./result%20of%20TestMethod4.png)
