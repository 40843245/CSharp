# CH4 -- an expression representing method call
## objectives
You will learn how to

+ create an expression representing method call

## CH4.1 -- create an expression representing method call
### `Expression.Call` static method
To create an expression representing method call,

just simply invoke `Expression.Call` static method.

About all overloads, see [`Expression.Call Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.call?view=net-8.0)

## examples
### example 1
Invoking the method

```
        /// <summary>
        /// illustrate how to create an expression representing a method call.
        /// </summary>
        public static void TestMethod26()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            MethodCallExpression methodCallExpression = Expression.Call(
                typeof(Console).GetMethod(
                      "WriteLine",
                      new Type [] { typeof(string) }
                ), 
                Expression.Constant("Hello World")
            );

            Expression.Lambda<Action>(methodCallExpression).Compile()();
        }
```

will output

```
In TestMethod26 method call,
Hello World
```

Analysis:

Let's dive into the code snippets first.

```
            MethodCallExpression methodCallExpression = Expression.Call(
                typeof(Console).GetMethod(
                      "WriteLine",
                      new Type [] { typeof(string) }
                ), 
                Expression.Constant("Hello World")
            );
```

+ In

```
                typeof(Console).GetMethod(
                      "WriteLine",
                      new Type [] { typeof(string) }
                ), 
```

`typeof(Console)` will return the type of `Console` class.

it use the overloads `Type.GetMethod(String name, Type[] types)`

    - the 0th argument `WriteLine` indicates that it's looking for the `WriteLine` `static` method.

    - the 1th argument `new Type [] { typeof(string) }` indicates that it's looking for one argument with string type.

it's looking for `Console.WriteLine` static method and expected one argument with string type is passed.

+ In `Expression.Constant("Hello World")`,

it creates an expression containing a constant with value `Hello World`.

And the `Hello World` will be passed into `Console.WriteLine` static method.

Second, the last statement will convert the expression to be a lamdba expression (in `Expression.Lambda<Action>(methodCallExpression)`), then compile it (in `Expression.Lambda<Action>(methodCallExpression).Compile()`), then execute the compiled delegate function (in `Expression.Lambda<Action>(methodCallExpression).Compile()();`).

Thus, the following code snippets

```
            MethodCallExpression methodCallExpression = Expression.Call(
                typeof(Console).GetMethod(
                      "WriteLine",
                      new Type [] { typeof(string) }
                ), 
                Expression.Constant("Hello World")
            );

            Expression.Lambda<Action>(methodCallExpression).Compile()();
```

will output

```
Hello World
```

## reference
### API docs
+ [`Expression.Call Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.call?view=net-8.0)
+ [`Type.GetMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethod?view=net-8.0)