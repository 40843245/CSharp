# CH2 -- get type of method info
## objectives
You will learn how to

+ get type which class defines this method

+ get member type of this method

+ get reflected type of this method

+ get return type of this method

## CH2.1 -- get type which class defines this method
### `DeclaringType` property (inherited from `MemberInfo`)
To get which class (`Type` type) that defines this member, 

one can access `DeclaringType` property.

For example,

```
// ... other import stament

namespace Example.DemoClass
{
    public class DemoClass1
    {
        public static void TestMethod2()
        {
            MethodBase currentMethod = MethodBase.GetCurrentMethod(); // will return current method Info
            Type declaringType = currentMethod.DeclaringType; // will return the class the defines of current method (here is `DemoClass1`)
        }
    }
}
```

## CH2.2 -- get member type of this method
### `MemberType` property (inherited from `MemberInfo`)
To get the id (enum value) that represents the type of member (such as `System.Reflection.MethodInfo` indicating it is a method),

one can access `MemberType` property.

For example,

```
// ... other import stament

namespace Example.DemoClass
{
    public class DemoClass1
    {
        public static void TestMethod2()
        {
            MethodBase currentMethod = MethodBase.GetCurrentMethod(); // will return current method Info
            MemberTypes memberType = currentMethod.MemberType; // will return an enum that represents type of member of current method.
        }
    }
}
```

## CH2.3 -- get reflected type of this method
### `ReflectedType` property (inherited from `MemberInfo`)
To get which instance that defines the given member, 

one can access `ReflectedType` property.

For example,

```
// ... other import stament

namespace Example.DemoClass
{
    public class DemoClass1
    {
        public static void TestMethod2()
        {
            MethodBase currentMethod = MethodBase.GetCurrentMethod(); // will return current method Info
            Type reflectedType = currentMethod.ReflectedType; // will return which instance that defines current method.
        }
    }
}
```

## CH2.4 -- get return type of this method
### `ReturnType` property (`MethodInfo`)
To get the return type of specific method, 

one can access `ReturnType` property of `MethodInfo` instance.

> [!CAUTION]
> NOTE that 
> 
> There are no `ReturnType` property in `MethodBase` class.
>
> Therefore, you have to cast `MethodBase` instance to `MethodInfo` instance,
>
> then check casts successfully, like this.
>
> ```
> MethodInfo methodInfo = methodBase as MethodInfo;
> if(methodInfo != null)
> {
>    Console.WriteLine("Return Type: {0}" , methodInfo);
> }
> ```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to 
        /// 
        /// + get declaring type of current running method
        /// 
        /// + get member type of current running method
        /// 
        /// + get reflected type of current running method 
        /// 
        /// + get return type of current running method
        /// 
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            MethodBase currentMethod = MethodBase.GetCurrentMethod(); // will return current method Info
            Type declaringType = currentMethod.DeclaringType; // will return the class the defines of current method (here is `DemoC;ass1`)

            MemberTypes memberType = currentMethod.MemberType; // will return an enum that represents type of member of current method.

            Type reflectedType = currentMethod.ReflectedType; // will return the type that is reflected by current method.

            // Removed the problematic line as 'MethodBase' does not have a 'ReturnType' property.
            // If you need the return type of the method, you should use 'MethodInfo' instead of 'MethodBase'.
            // Example:
            MethodInfo methodInfo = currentMethod as MethodInfo;

            Console.WriteLine("Current Method Name: {0}" , currentMethod.Name);
            Console.WriteLine("Which class declares this member? {0}" , declaringType?.Name ?? "null");
            Console.WriteLine("Id that representing the type of member: {0}" , memberType);

            if(methodInfo != null)
            {
                Console.WriteLine("Return Type of {0} method: {1}" , methodInfo.Name,methodInfo.ReturnType.FullName);
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod2 method call,
Current Method Name: TestMethod2
Which class declares this member? DemoClass1
Id that representing the type of member: Method
Return Type of TestMethod2 method: System.Void
End of TestMethod2 method call,
```

## reference
### API docs
+ [`MethodBase.GetCurrentMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.getcurrentmethod?view=net-9.0)

+ [`MemberInfo.DeclaringType Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo.declaringtype?view=net-9.0)

+ [`MemberInfo.MemberType Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo.membertype?view=net-9.0)

+ [`MemberInfo.ReflectedType Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo.reflectedtype?view=net-9.0)

+ [`MethodInfo.ReturnType Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.returntype?view=net-9.0)
