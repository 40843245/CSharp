# CH2 -- output text on console
## objectives
You will learn how to

+ output text on console

## CH2.1 -- output text on console
To output text on console, there are few static method we can invoke.

+ `Console.Write`: 

    - ~~With zero arguments:~~

    - With one argument: convert it to `string` then output it on the console that starts from the current cursors position.

    - With many arguments: In zero-based index system, convert all arguments except zero argument into `object` 
    
      and then format zero argument with all arguments except zero argument.

+ `Console.WriteLine`:

Behaves similar to `Console.Write` static method, but it output a break line (`\n`), making the current cursor ends at the beginning of next line.

Additionally, `Console.WriteLine` also accepts zero arguments

    - With zero arguments: only output break line.

    - With one argument: do same behave of `Console.Write` and then output break line.

    - With many arguments: do same behave of `Console.Write` and then output break line.

> [!TIP]
> Think of this
> 
> `Console.WriteLine();` is equivalent to 
>
> `Console.Write("\n");`

> [!TIP]
> Think of this
> 
> `Console.WriteLine(str);` is equivalent to 
> 
> + `Console.WriteLine(str+"\n");` and
> 
> + `Console.Write(str);`  
>   
>   `Console.WritLine();`
>
> it can proven by the definition of `Console.WritLine` and `Console.Write`.

> [!TIP]
> Think of this
> 
> `Console.Write(inst);` is equivalent to
> 
> + `Console.Write(inst.ToString());`

> [!TIP]
> Think of this
> 
> `Console.Write(formattingString, arg0, arg1,...);` is equivalent to
> 
> + `Console.Write(string.Format(formattingString,arg0,arg1));`

> [!CAUTION]
> Watch out for many arguments.
>
> `Console.Write` can NOT accept zero argument. 
>
> Thus, if one invokes `Console.Write();`, it will cause compiler error.

> [!CAUTION]
> Watch out for many arguments.
>
> If one passed many arguments in either `Console.Write` or `Console.WriteLine` static method call,
>
> the zero argument MUST be `string` struct. (except the special use case `public static void Write(char[] buffer, int index, int count);`)
>
> Otherwise, it will cause compile error.

### overloads
For more overloads, see API docs.

+ [`Console.WriteLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.writeline?view=net-9.0)

+ [`Console.Write Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.write?view=net-9.0)

## reference
### API docs
+ [`Console.WriteLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.writeline?view=net-9.0)

+ [`Console.Write Method`](https://learn.microsoft.com/en-us/dotnet/api/system.console.write?view=net-9.0)

