# CH17 -- an expression about exception handling
## objectives
You will learn how to

+ create an expression that represents `throw` statement 
+ create an expression that represents `rethrow` statement
+ create an expression that represents `catch` block 
+ create an expression that represents `try` block or `try` block combined with `catch` block
+ create an expression that represents `try` block combined with `finally` block
+ create an expression that represents `try` block combined with `catch` block then combined with `finally` block

## CH17.1 -- create an expression about `throw` statement
### `Expression.Throw` static method
To create an expression that represents `throw` statement, just simply invoke `Expression.Throw` static method.

basic structure:

```
UnaryExpression unaryExpression = Expression.Throw(Expression.Constant(new DivideByZeroException())); // inside the `Expression.Constant` method call, instantiate an exception with type `Exception` class or its derived class.
```

or

```
UnaryExpression unaryExpression = Expression.Throw(Expression.Constant(new DivideByZeroException()),typeof(int)); // inside the `Expression.Constant` method call, instantiate an exception with type `Exception` class or its derived class.
```


Think of this:

When the expression

```
UnaryExpression unaryExpression = Expression.Throw(Expression.Constant(new DivideByZeroException())); // inside the `Expression.Constant` method call, instantiate an exception with type `Exception` class or its derived class.
```

is compiled, 

it will become `throw` statement, as follows.

```
throw new DivideByZeroException();
```

## CH17.2 -- create an expression about `rethrow` statement
### `Expression.Rethrow` static method
To create an expression that represents `rethrow` statement, just simply invoke `Expression.Rethrow` static method.

basic structure:

```
UnaryExpression unaryExpression = Expression.Rethrow();
```

or

```
UnaryExpression unaryExpression = Expression.Rethrow(typeof(int));
```

Think of this:

When the expression

```
UnaryExpression unaryExpression = Expression.Rethrow();
```

is compiled, 

it will become `rethrow` statement, as follows.

```
rethrow;
```

## CH17.3 -- create an expression about `catch` block
### `Expression.Catch` static method
To create an expression that represents `catch` block, just simply invoke `Expression.Catch` static method.

basic structure:

```
CatchBlock catchBlock =
      Expression.Catch(
            typeof(DivideByZeroException), // type of Exception that will be fetched in `catch` block
            // ... logic inside `catch` block
            // ... return type of last expression
      );
```

Think of this:

When the expression

```
CatchBlock catchBlock =
      Expression.Catch(
            typeof(DivideByZeroException), // type of Exception that will be fetched in `catch` block
            // ... logic inside `catch` block
            // ... return type of last expression
      );
```

is compiled, 

it will become `catch` block, as follows.

```
catch(DivideByZeroException ex)
{
    // ... logic inside `catch` block
    // ... return type of last expression
}
```

## CH17.4 -- create an expression that represents `try` block or `try` block combined with `catch` block
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

basic structure:

```
TryExpression tryExpression =
    Expression.TryCatch(
      /// `try` block
      // ... logic inside `try` block
      // ... return type of last expression
      // Watch out that return type of last expression here MUST be same of that in `catch` block.
      ,
      /// `catch` block
      // can be null
      Expression.Catch(
            typeof(DivideByZeroException), // type of `Exception` that will be fetched in `catch` block
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

Think of this:

When it is

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
    );
```

compiled, it will become `try`-`catch` block as follows.

```
try{
    // ... logic inside `try` block
    // ... return type of last expression
    // Watch out that return type of last expression here MUST be same of that in `catch` block.
}catch(DivideByZeroException ex){
    // ... logic inside `catch` block
    // ... return type of last expression
}
```

## CH17.5 -- create an expression that represents `try` block combined with `finally` block
### `Expression.TryFinally` static method
To create an expression that represents `try` block combined with `finally` block, 

just simply invoke `Expression.TryFinally` static method.

basic structure:

```
TryExpression tryExpression =
                Expression.TryFinally(
                        /// `try` block
                        // ... logic here
                        // ... return type of last expression
                        // return type of last expression here MUST be same as that in other block
                        ,
                        /// `finally` block
                        // ... logic here
                        // ... return type of last expression
                );
```

Think of this:

After compiling it

```
            ParameterExpression nameVariableExpression = Expression.Variable(typeof(string) , "name");

            ParameterExpression reachedVariableExpression = Expression.Variable(typeof(bool) , "reached");

            TryExpression tryExpression =
                Expression.TryFinally(
                    Expression.Block(
                        Expression.Block(
                            new ParameterExpression [ ] { reachedVariableExpression } ,
                            Expression.Assign(
                                reachedVariableExpression ,
                                Expression.Constant(false)
                            )
                        ),
                        Expression.Block(
                            new ParameterExpression [ ] { nameVariableExpression } ,
                            Expression.Assign(
                                nameVariableExpression ,
                                Expression.Constant("Yazawa Nico")
                            )
                        ),
                        Expression.Constant("Try block")
                    ) ,
                    Expression.Block(
                        Expression.Block(
                            new ParameterExpression [ ] { nameVariableExpression } ,
                            Expression.Assign(
                                nameVariableExpression ,
                                Expression.Constant("Ayase Eli")
                            )
                        ),
                        Expression.Block(
                            new ParameterExpression [ ] { reachedVariableExpression } ,
                            Expression.Assign(
                                reachedVariableExpression ,
                                Expression.Constant(true)
                            )
                        ),
                        Expression.Constant("Finally block")
                    )
                );
```

it will become

```
bool reached;
string name;

try
{
      reached = false;
      name = "Yazawa Nico";
      return "Try block";
}
finnally
{
      name = "Ayase Eli";
      reached = true;
      return "Finally block";
}
```

## CH17.6 -- create an expression that represents `try` block combined with `catch` block then combined with `finally` block
### `Expression.TryCatchFinally` static method
To create an expression that represents `try` block combined with `catch` block then combined with `finally` block,

just simply invoke `Expression.TryCatchFinally` static method.

basic structure:

```
TryExpression tryCatchExpression =
                Expression.TryCatchFinally(
                        /// `try` block 
                        // ... logic here
                        // ... return type of last expression
                        // return type of last expression here MUST be same as that in other block
                        ,
                        /// `finnally` block
                        // ... logic here
                        // ... return type of last expression
                        ,
                        /// `catch` block
                    Expression.Catch(
                        typeof(DivideByZeroException), // type of the `Exception` to be fetched in `catch` block.
                        // ... logic here
                        // ... return type of last expression
                    )
                );
```

## examples
### example 1
Invoking this method

```
        /// <summary>
        /// illustrate how to create an expression with `try` block and `catch` block.
        /// </summary>
        public static void TestMethod20()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

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
In TestMethod20 method call,
try { ... }
Body:{ ... }
Its Fault:
It has 1 handlers.
0th handler:catch (DivideByZeroException) { ... }
Finally Block:
Catch block

```

### example 2
Invoking this method will throw exception since one creates an expression that throws exception.

```
        /// <summary>
        /// illustrate how to create an expression with `try` block and `finally` block.
        /// 
        /// In this example, one throws an Excpetion in try block and NOT be caught.
        /// 
        /// Thus, when the expression is compiled then is executed, it throws an exception.
        /// </summary>
        public static void TestMethod21()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ParameterExpression variableExpression = Expression.Variable(typeof(string) , "name");

            BinaryExpression assignmentExpression = Expression.Assign(
                variableExpression,
                Expression.Constant("Yazawa Nico")
            );

            BlockExpression blockExpression = Expression.Block(
                new ParameterExpression [ ] { variableExpression },
                assignmentExpression
            );

            TryExpression tryExpression =
                Expression.TryFinally(
                    Expression.Block(
                        Expression.Throw(
                            Expression.Constant(new DivideByZeroException())
                        ) ,
                        Expression.Constant("Try block")
                    ),
                    Expression.Block(
                        blockExpression,
                        Expression.Constant("Finally block")
                    )
                );

            string infoText = tryExpression.GetInfo();
            Console.WriteLine(infoText);
        }
```

The output:

<img width="869" alt="image" src="https://github.com/user-attachments/assets/6ab1cd82-12ca-45c6-8e57-7a5ce85bbe16" />

<img width="638" alt="image" src="https://github.com/user-attachments/assets/b7c8f039-a2b3-49ee-9950-59254b61d48b" />

### example 3
Invoking following method

```
        /// <summary>
        /// illustrate how to create an expression with `try` block and `finally` block.
        /// 
        /// In this example, one does NOT throw exceptions in `try` block.
        /// 
        /// Thus, when the expression is compiled then is executed, 
        /// 
        /// `finally` block will be executed.
        /// </summary>
        public static void TestMethod22()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ParameterExpression nameVariableExpression = Expression.Variable(typeof(string) , "name");

            ParameterExpression reachedVariableExpression = Expression.Variable(typeof(bool) , "reached");

            TryExpression tryExpression =
                Expression.TryFinally(
                    Expression.Block(
                        Expression.Block(
                            new ParameterExpression [ ] { reachedVariableExpression } ,
                            Expression.Assign(
                                reachedVariableExpression ,
                                Expression.Constant(false)
                            )
                        ),
                        Expression.Block(
                            new ParameterExpression [ ] { nameVariableExpression } ,
                            Expression.Assign(
                                nameVariableExpression ,
                                Expression.Constant("Yazawa Nico")
                            )
                        ),
                        Expression.Constant("Try block")
                    ) ,
                    Expression.Block(
                        Expression.Block(
                            new ParameterExpression [ ] { nameVariableExpression } ,
                            Expression.Assign(
                                nameVariableExpression ,
                                Expression.Constant("Ayase Eli")
                            )
                        ),
                        Expression.Block(
                            new ParameterExpression [ ] { reachedVariableExpression } ,
                            Expression.Assign(
                                reachedVariableExpression ,
                                Expression.Constant(true)
                            )
                        ),
                        Expression.Constant("Finally block")
                    )
                );

            string infoText = tryExpression.GetInfo();
            Console.WriteLine(infoText);
        }
```

where

`TryCatchExpressionsExtensionMethods.GetInfo` extension method is defined in `TryCatchExpressionsExtensionMethods` static class.

`TryCatchExpressionsExtensionMethods` static class in `ExpressionExtensionMethods.cs`

```
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
                    var result = string.Format(formattingString , counter , "handler" , handler?.GetSimpleInfo());
                    counter++;
                    return result;
                }
            );
            
            stringBuilder.AppendFormat("{0}\n" , tryExpression.ToString());
            stringBuilder.AppendFormat("Body:{0}\n" , tryExpression.Body.GetSimpleInfo());
            stringBuilder.AppendFormat("Its Fault:{0}\n" , tryExpression.Fault?.GetSimpleInfo());
            stringBuilder.AppendFormat("It has {0} handlers.\n",handlers.Count);
            stringBuilder.Append(string.Join(System.Environment.NewLine , textList));
            stringBuilder.Append(handlers.Count > 0 ? System.Environment.NewLine : string.Empty);
            stringBuilder.AppendFormat("Finally Block:{0}\n" , tryExpression.Finally?.GetSimpleInfo());
            stringBuilder.AppendLine("After executing the compiled delegate function,");
            stringBuilder.AppendFormat("{0}\n" , compiledDelegateFunction.Invoke());
            return stringBuilder.ToString();
        }
    }
```

and 

`ExpressionsExtensionMethods.GetSimpleInfo` extension method is defined in `ExpressionsExtensionMethods` static class.

`ExpressionsExtensionMethods` static class in `ExpressionsExtensionMethods.cs`

```
    public static class ExpressionsExtensionMethods
    {
        public static string GetSimpleInfo(
            this Expression expression
        )
        {
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendLine(expression.Type.ToString());
            stringBuilder.AppendLine(expression.NodeType.ToString());
            stringBuilder.AppendLine(expression.CanReduce.ToString());
            return stringBuilder.ToString();
        }
    }
```

and

`CatchBlocksExtensionMethods.GetSimpleInfo` extension method is defined in `CatchBlocksExtensionMethods` static class.

`CatchBlocksExtensionMethods` static class in `ExpressionsExtensionMethods.cs`.

```
    public static class CatchBlocksExtensionMethods
    {
        public static string GetSimpleInfo(
            this CatchBlock catchBlock
        )
        {
            var compiledDelegateFunction = Expression.Lambda<Func<object>>(catchBlock.Body).Compile();
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendLine(catchBlock.Test.Name);
            stringBuilder.AppendLine(compiledDelegateFunction.ToString());
            return stringBuilder.ToString();
        }
    }
```

and 

`Apply` extension method is defined in `HighOrderFunctionsExtensionMethods` static class.

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
In TestMethod22 method call,
try { ... }
Body:System.String
Block
False

Its Fault:
It has 0 handlers.
Finally Block:System.String
Block
False

After executing the compiled delegate function,
Try block
```

### example 3
Invoking this method 

```
        /// <summary>
        /// `Expression.TryCatchFinally` example.
        /// </summary>
        public static void TestMethod23()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            TryExpression tryCatchExpression =
                Expression.TryCatchFinally(
                    Expression.Block(
                        Expression.Throw(Expression.Constant(new DivideByZeroException())) ,
                        Expression.Constant("Try block")
                    ) ,
                    Expression.Call(typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) }) , Expression.Constant("Finally block")) ,
                    Expression.Catch(
                        typeof(DivideByZeroException) ,
                        Expression.Constant("Catch block")
                    )
                );

            // The following statement first creates an expression tree,
            // then compiles it, and then runs it.
            // If the exception is caught,
            // the result of the TryExpression is the last statement
            // of the corresponding catch statement.
            Console.WriteLine(Expression.Lambda<Func<string>>(tryCatchExpression).Compile()());
        }
```

will output following

```
In TestMethod23 method call,
Finally block
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
