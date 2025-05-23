# CH14 -- an expression about exception handling
## objectives
You will learn how to

+ create an expression about exception handling

## CH14.1 -- an expression about exception handling
### `Expression.TryCatch` static method
To create an expression (`TryExpression`) that represents a try-catch block with zero or more catach statement and niether a fault nor finally block,

one can invoke `Expression.TryCatch` static method.

Let's look at its syntax

<img width="457" alt="image" src="https://github.com/user-attachments/assets/bb5f99c6-0a77-40c4-826a-bb3991ddc451" />

```
public static System.Linq.Expressions.TryExpression TryCatch(
    System.Linq.Expressions.Expression body,
    params System.Linq.Expressions.CatchBlock[]? handlers
);
```

Basic usage:

```
TryExpression tryExpression =
    Expression.TryCatch(
      Expression.Block(
            // ... logic inside `try` block
            // ... return type of last expression
            // Watch out that return type of last expression here MUST be same of that in `catch` block.
      ),
      // can be null
      Expression.Catch(
            typeof(DivideByZeroException), // type of Exception that will be fetched in `catch` block
            // ... logic inside `catch` block
            // ... return type of last expression
      )
      // ... and more expression about `catch` block (separated with comma)
    );
```

> [!IMPORTANT]
> The `try-catch` block must have same return type.
>
> For example, inside a function or method.
>
> ```
> try
> {
>  // ... logic
>  return true;
> }catch(Exception ex)
> {
>  // ... exception handling
>    return false;
> }
> ```

> [!CAUTION]
> Watch out
>
> the consisency of return type in last expression between an expression representing `try` block and that representing `catch` block.
>
> Otherwise, it may throw an exception at runtime when trying to compile the `TryExpression` instance.
>
> The reason why is that the `try-catch` block must have same return type (which has been discussed before)

## example
### example 1
Invoking this method

```
        /// <summary>
        /// illustrate how to create an expression with `try` block and `catch` block.
        /// </summary>
        public static void TestMethod20()
        {
            TryExpression tryCatchExpression =
                Expression.TryCatch(
                    Expression.Block(
                        Expression.Throw(
                            Expression.Constant(new DivideByZeroException())
                        ),
                        Expression.Constant("Try block")
                    ) ,
                    Expression.Catch(
                        typeof(DivideByZeroException) ,
                        Expression.Constant("Catch block")
                    )
                );

            string infoText = tryCatchExpression.GetInfo();
            Console.WriteLine(infoText);
        }
```

where 

`TryExpression.GetInfo` extension method is defined in `TryCatchExpressionsExtensionMethods` static class.

`TryCatchExpressionsExtensionMethods` static class in `ExpressionsExtensionMethods.cs`

```
using static Example.Extensions.ExtensionMethods.HighOrderFunctionsExtensionMethods.HighOrderFunctionsExtensionMethods; // for `Apply` extension method

    public static class TryCatchExpressionsExtensionMethods
    {
        public static string GetInfo(
            this TryExpression tryExpression
        )
        {
            var handlers = tryExpression.Handlers;
            var compiledDelegateFunction = Expression.Lambda<Func<string>>(tryExpression).Compile();
            StringBuilder stringBuilder = new StringBuilder();

            List<string> textList = new List<string>();
            string formattingString = "{0}th {1}:{2}";
            int counter = 0;

            counter = 0;
            textList = handlers.Apply<CatchBlock , string>(
                handler =>
                {
                    var result = string.Format(formattingString , counter , "handler" , handler.ToString());
                    counter++;
                    return result;
                }
            );
            
            stringBuilder.AppendFormat("{0}\n" , tryExpression.ToString());
            stringBuilder.AppendFormat("Body:{0}\n" , tryExpression.Body);
            stringBuilder.AppendFormat("Its Fault:{0}\n" , tryExpression.Fault);
            stringBuilder.AppendFormat("It has {0} handlers.\n",handlers.Count);
            stringBuilder.Append(string.Join(System.Environment.NewLine , textList));
            stringBuilder.Append(handlers.Count > 0 ? System.Environment.NewLine : string.Empty);
            stringBuilder.AppendFormat("Finally Block:{0}\n" , tryExpression.Finally);
            stringBuilder.AppendFormat("{0}\n" , compiledDelegateFunction.Invoke());
            return stringBuilder.ToString();
        }
    }
```

`Apply` extension method is defined as follows

`HighOrderFunctionsExtensionMethods.cs`

```
using System;
using System.Collections.Generic;

namespace Example.Extensions.ExtensionMethods.HighOrderFunctionsExtensionMethods
{
    public static partial class HighOrderFunctionsExtensionMethods
    {
        public static List<TFunctionResult> Apply<TElement, TFunctionResult>(
            this ICollection<TElement> collection,
            Func<TElement , TFunctionResult> func
        )
        {
            List<TFunctionResult> list = new List<TFunctionResult>();
            foreach(var coll in collection)
            {
                var result = func.Invoke(coll);
                list.Add(result);
            }
            return list;
        }
    }
}
```

It will output following

```
try { ... }
Body:{ ... }
Its Fault:
It has 1 handlers.
0th handler:catch (DivideByZeroException) { ... }
Finally Block:
Catch block

```

## reference
### API docs
+ [`Expression.Throw Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.throw?view=net-8.0)
+ [`Expression.Rethrow Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.rethrow?view=net-8.0)
+ [`Expression.TryCatch(Expression, CatchBlock[]) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.trycatch?view=net-8.0)
+ [`Expression.TryCatchFinally(Expression, Expression, CatchBlock[]) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.trycatchfinally?view=net-8.0)
+ [`Expression.TryFinally(Expression, Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.tryfinally?view=net-8.0)
+ [`TryExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.tryexpression?view=net-8.0)
+ [`CatchBlock Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.catchblock?view=net-8.0)
