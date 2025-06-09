# CH5 -- an expression about invoking an expression with generic type
## objectives
You will learn how to

+ create an expression about invoking an expression with generic type

## CH5.1 -- create an expression about invoking an expression with generic type
### `Expression.Invoke` static method
To create an expression about invoking an expression with generic type,

just simply invoke `Expression.Invoke` static method.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to create an expression about invoking an expression with generic type.
        /// </summary>
        public static void TestMethod27()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            System.Linq.Expressions.Expression<Func<int , int , bool>> largeSumTest =
                (num1 , num2) => (num1 + num2) > 1000;

            // Create an InvocationExpression that represents applying
            // the arguments '539' and '281' to the lambda expression 'largeSumTest'.
            InvocationExpression invocationExpression =
                Expression.Invoke(
                    largeSumTest ,
                    Expression.Constant(539) ,
                    Expression.Constant(281)
                );

            string infoText = invocationExpression.GetInfo();
            Console.WriteLine(infoText);
        }
```

where

`InvocationExpression.GetInfo` extension method is defined in `InvocationExpressionExtensionMethods` static class.

`InvocationExpressionExtensionMethods` static class in `ExpressionsExtensionMethods.cs`

```
    public static class InvocationExpressionExtensionMethods
    {
        public static string GetInfo(
            this InvocationExpression invocationExpression
        )
        {
            StringBuilder stringBuilder = new StringBuilder();
            List<string> textList = new List<string>();
            string formattingString = "{0}th {1}:{2}";
            int counter = 0;

            var arguments = invocationExpression.Arguments;

            counter = 0;
            textList = arguments.Apply<Expression , string>(
                argument =>
                {
                    var result = 
                        string.Format(
                            formattingString,
                            counter,
                            "argument",
                            argument.GetSimpleInfo()
                        );
                    counter++;
                    return result;
                }
            );
            stringBuilder.AppendLine(invocationExpression.ToString());
            stringBuilder.AppendFormat("Expression that are applied:\n{0}\n",invocationExpression.Expression.GetSimpleInfo());
            stringBuilder.AppendFormat("There are {0} arguments.\n",arguments.Count);
            stringBuilder.Append(string.Join(System.Environment.NewLine,textList));
            stringBuilder.Append(arguments.Count > 0 ? System.Environment.NewLine : string.Empty);
            stringBuilder.AppendLine(invocationExpression.CanReduce.ToString());
            return stringBuilder.ToString();
        }
```

and 

`Expression.GetSimpleInfo` extension method is defined in `ExpressionsExtensionMethods` static class

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

It will output

```
In TestMethod27 method call,
Invoke((num1, num2) => ((num1 + num2) > 1000), 539, 281)
Expression that are applied:
System.Func`3[System.Int32,System.Int32,System.Boolean]
Lambda
False

There are 2 arguments.
0th argument:System.Int32
Constant
False

1th argument:System.Int32
Constant
False

False

```

## reference
### API docs
+ [`Expression.Invoke Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.invoke?view=net-8.0)
+ [`InvocationExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.invocationexpression?view=net-8.0)