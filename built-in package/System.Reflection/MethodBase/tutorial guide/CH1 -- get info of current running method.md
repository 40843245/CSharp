# CH1 -- get info of current running method
## objectives
You will learn how to

+ get info of current running method

## CH1.1 -- get info of current running method
### `MethodBase.GetCurrentMethod` static method
Invoking `MethodBase.GetCurrentMethod` static method will return `MethodBase` 

representing info of current running method.

To get name of current running method, just use reflection.

```
string currentMethodName = MethodBase.GetCurrentMethod().Name;
```

> [!CAUTION]
> There is no way to get full name of current running method
>
> The following code will produce a compiler error. 
>
> ```
> string currentMethodName = MethodBase.GetCurrentMethod().FullName; // <- error.
> ```


Use case:

I want to get current running method name and print it in console, 

making me easier to know which current method is running (as I always do in projects for tutorial).

Then I wrote something like following example 1.


## examples
### example 1
#### main code
Invoking following method
```
// ... other import stament

namespace Example.DemoClass
{
    public class DemoClass1
    {
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().FullName); // <- error.

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);

            // Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().FullName); // <- error.
        }
    }
}

```

will output following

```
In TestMethod1 method call,
End of TestMethod1 method call,
```

## reference
### API docs
+ [`MethodBase Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase?view=net-9.0)

+ [`MethodBase.GetCurrentMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.getcurrentmethod?view=net-9.0)