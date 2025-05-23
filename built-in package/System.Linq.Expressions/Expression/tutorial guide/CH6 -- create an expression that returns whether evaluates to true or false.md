# CH6 -- create an expression that returns whether evaluates to true or false
## objectives
You will learn how to

+ create an expression that returns whether evaluates to true or false

## CH6.1 -- create an expression that returns whether evaluates true or false
### `Expression.IsTrue` static method
You can invoke `Expression.IsTrue` static method to create an expression that returns whether evaluates to true.

Syntax:

```
public static System.Linq.Expressions.UnaryExpression IsTrue(
  System.Linq.Expressions.Expression expression
);
```

or

```
public static System.Linq.Expressions.UnaryExpression IsTrue(
  System.Linq.Expressions.Expression expression,
  System.Reflection.MethodInfo? method
);
```

For example:

```
Expression expressionConstantTrue = Expression.Constant(true);
Expression.IsTrue(expressionConstantTrue);
```

### `Expression.IsFalse` static method
You can invoke `Expression.IsFalse` static method to create an expression that returns whether evaluates to false.

Syntax:

```
public static System.Linq.Expressions.UnaryExpression IsFalse(
  System.Linq.Expressions.Expression expression
);
```

or

```
public static System.Linq.Expressions.UnaryExpression IsFalse(
  System.Linq.Expressions.Expression expression,
  System.Reflection.MethodInfo? method
);
```

For example:

```
Expression expressionConstantFalse = Expression.Constant(false);
Expression.IsFalse(expressionConstantFalse);
```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate to check the `Expression` with boolean data type is true or false.
        /// </summary>
        public static void TestMethod6()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Expression expressionConstantTrue = Expression.Constant(true);
            Expression expressionConstantFalse = Expression.Constant(false);

            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();

            unaryExpressions.Add(
                Expression.IsTrue(
                    expressionConstantTrue
                )
            );

            unaryExpressions.Add(
                Expression.IsTrue(
                    expressionConstantFalse
                )
            );

            unaryExpressions.Add(
                Expression.IsFalse(
                    expressionConstantTrue
                )
            );

            unaryExpressions.Add(
                Expression.IsFalse(
                    expressionConstantFalse
                )
            );

            unaryExpressions.ForEach(
                (unaryExpression) => 
                {
                    string infoText = unaryExpression.GetInfo();
                    Console.WriteLine(infoText);
                }
            );
        }
```

where

`UnaryExpression.GetInfo` extension method is defined in `UnaryExpressionsExtensionMethods` static class

`ExpressionsExtensionMethods.cs`

```
    public static class UnaryExpressionsExtensionMethods
    {
        public static string GetInfo(
            this UnaryExpression unaryExpression
        )
        {
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendFormat("unaryExpression.IsLifted:{0}\n" , unaryExpression.IsLifted);
            stringBuilder.AppendFormat("unaryExpression.IsLiftedToNull:{0}\n" , unaryExpression.IsLiftedToNull);
            stringBuilder.AppendFormat("unaryExpression.Operand:{0}\n" , unaryExpression.Operand);
            return stringBuilder.ToString();
        }
    }
```

It will output following

```
In TestMethod6 method call,
unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:True

unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:False

unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:True

unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:False
```

## reference
### API docs
+ [`Expression.IsTrue Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.istrue?view=net-8.0)
+ [`Expression.IsFalse Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.isfalse?view=net-8.0)
