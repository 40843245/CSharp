# CH15 -- create a dynamic expression
## objectives
You will learn how to

+ create a dynamic expression

## CH15.1 -- create a dynamic expression
### `Expression.Dynamic` static method

Step 1:

Defines a class and its methods (can be any name you want) used as dynamic caller in `Expression.Dynamic` static method.

Code snippets for example,

Step 2:

Instantiate the defined class.

Step 3:

Create three expressions, each expression contains a parameter.

+ one expression related to instance for dynamic call.
+ one expression related to method name for dynamic call.
+ one expression related to arguments/parameter for dynamic call.

> [!TIP]
> To create an expression containing a parameter,
>
> you can invoke `Expression.Parameter` static method.
>
> For more details, see `CH2`.

Step 4:

To create a new CSharp invoke member binder, 

we need to instantiate `System.Runtime.CompilerServices.CallSiteBinder` instance by `Microsoft.CSharp.RuntimeBinder.Binder.InvokeMember` factory method.

```
System.Runtime.CompilerServices.CallSiteBinder invokeBinder = Microsoft.CSharp.RuntimeBinder.Binder.InvokeMember(
//... arguments passed here
);
```

Code snippets for example,

```
            CallSiteBinder invokeBinder = Binder.InvokeMember(
                CSharpBinderFlags.None , // No special flags
                "DynamicMethod" ,        // This name isn't actually used for the method being invoked,
                                         // but for the CallSite. It's often just a placeholder.
                null ,                   // Type arguments for generic methods (none here)
                currentClassType ,        // The context type for the binder
                new CSharpArgumentInfo [ ]
                {
                    CSharpArgumentInfo.Create(CSharpArgumentInfoFlags.None, null), // For the instance
                    CSharpArgumentInfo.Create(CSharpArgumentInfoFlags.UseCompileTimeType | CSharpArgumentInfoFlags.Constant, null), // For the method name (treated as constant)
                    CSharpArgumentInfo.Create(CSharpArgumentInfoFlags.None, null)  // For the arguments array
                });
```

let's dive into `Microsoft.CSharp.RuntimeBinder.Binder.InvokeMember`.

first, let's take look at syntax of `Microsoft.CSharp.RuntimeBinder.Binder.InvokeMember`.

Syntax:

<img width="452" alt="image" src="https://github.com/user-attachments/assets/26bfc67c-824d-409c-a61d-1e91ba9f29db" />

```
public static System.Runtime.CompilerServices.CallSiteBinder InvokeMember(
  Microsoft.CSharp.RuntimeBinder.CSharpBinderFlags flags,
  string name,
  System.Collections.Generic.IEnumerable<Type>? typeArguments,
  Type? context,
  System.Collections.Generic.IEnumerable<Microsoft.CSharp.RuntimeBinder.CSharpArgumentInfo>? argumentInfo
);
```

then let's explain each parameter of `Microsoft.CSharp.RuntimeBinder.Binder.InvokeMember`.

<img width="484" alt="image" src="https://github.com/user-attachments/assets/10dd22e9-500d-4c52-a117-3ab18473eca3" />

+ `Microsoft.CSharp.RuntimeBinder.CSharpBinderFlags` flags: The flags used when initializing the binder.
+ `string` name: The name of the member to invoke.
+ `System.Collections.Generic.IEnumerable<Type>?` typeArguments: The list of type arguments specified for this invoke.
+ `Type?` context: The Type that indicates where this operation is used.
+ `System.Collections.Generic.IEnumerable<Microsoft.CSharp.RuntimeBinder.CSharpArgumentInfo>?` argumentInfo: a list of arguments used in this operation.

For more details, see [`Binder.InvokeMember Method`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.csharp.runtimebinder.binder.invokemember?view=net-9.0)

Step 5:

Then we can invoke `Expression.Dynamic` static method to create an dynamic expression.

Code snippets for example,

```
            Expression dynamicCallExpression = Expression.Dynamic(
                invokeBinder ,
                typeof(object) , // Return type of the dynamic operation
                Expression.Constant(dynamicMethodCaller , typeof(object)) , // The instance
                Expression.Constant("Greet" , typeof(string)) , // The method name
                Expression.Constant(new object [ ] { "World" } , typeof(object [ ])) // The arguments
            );
```

Step 6:

Then compile it by `Compile` instance call. 

After that, it will be a delegate function.

> [!CAUTION]
> For compatibility to arguments passed in dynamic call, use `object` as generic type. As follows.
>
> ```
> Func<object> dynamicInvoker = Expression.Lambda<Func<object>>(dynamicCallExpression).Compile();
> ```

Step 7:

Then execute it by `Invoke` call.

```
object result1 = dynamicInvoker.Invoke();
```

## examples
### example 1
Invoking this method

```
        /// <summary>
        /// illustrate how to create a dynamic expression.
        /// compile it then execute it. 
        /// </summary>
        public static void TestMethod17()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Type currentClassType = MethodBase.GetCurrentMethod().DeclaringType;

            DynamicMethodCaller dynamicMethodCaller = new DynamicMethodCaller();
            ParameterExpression instanceParam = Expression.Parameter(typeof(object) , "instance");
            ParameterExpression methodNameParam = Expression.Parameter(typeof(string) , "methodName");
            ParameterExpression argsParam = Expression.Parameter(typeof(object [ ]) , "args");

            // Create a C# CallSite for the dynamic operation
            // This is where the magic of the DLR happens.
            // Binder.InvokeMember is used to perform dynamic method invocation.
            CallSiteBinder invokeBinder = Binder.InvokeMember(
                CSharpBinderFlags.None , // No special flags
                "DynamicMethod" ,        // This name isn't actually used for the method being invoked,
                                         // but for the CallSite. It's often just a placeholder.
                null ,                   // Type arguments for generic methods (none here)
                currentClassType ,        // The context type for the binder
                new CSharpArgumentInfo [ ]
                {
                    CSharpArgumentInfo.Create(CSharpArgumentInfoFlags.None, null), // For the instance
                    CSharpArgumentInfo.Create(CSharpArgumentInfoFlags.UseCompileTimeType | CSharpArgumentInfoFlags.Constant, null), // For the method name (treated as constant)
                    CSharpArgumentInfo.Create(CSharpArgumentInfoFlags.None, null)  // For the arguments array
                });

            Expression dynamicCallExpression = Expression.Dynamic(
                invokeBinder ,
                typeof(object) , // Return type of the dynamic operation
                Expression.Constant(dynamicMethodCaller , typeof(object)) , // The instance
                Expression.Constant("Greet" , typeof(string)) , // The method name
                Expression.Constant(new object [ ] { "World" } , typeof(object [ ])) // The arguments
            );

            Func<object> dynamicInvoker = Expression.Lambda<Func<object>>(dynamicCallExpression).Compile();

            object result1 = dynamicInvoker.Invoke();
            Console.WriteLine($"Result of Greet: {result1}");
        }
```

