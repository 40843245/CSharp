# CH2 -- create an expression containing a variable, a parameter, or a constant
## objectives
You will learn

+ an expression containing a variable
+ an expression containing a parameter
+ an expression containing a constant
+ an expression about assigment operator

## CH2.1 -- an expression containing a variable
### `Expression.Variable` static method call
You can create an expression contnaining a variable using `Expression.Variable` static method call.

```
ParameterExpression intParameterExpression1 = Expression.Variable(typeof(int) , "x");
```

In above code snippets, an expression containing a variable will be created. 

The variable name is `x` with int type.

For more details, see [`Expression.Variable Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.variable?view=net-8.0)

## CH2.2 -- an expression containing a parameter
### `Expression.Parameter` static method call
You can create an expression contnaining a variable using `Expression.Parameter` static method call.

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
```

In above code snippets, an expression containing a parameter will be created. 

The parameter name is `x` with int type.

For more details, see [`Expression.Parameter Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.parameter?view=net-8.0)

## CH2.3 -- an expression containing a constant
### `Expression.Constant` static method call
You can create an expression contnaining a constant using `Expression.Constant` static method call.

```
ConstantExpression expressionConstantOne = Expression.Constant(1);
```

In above code snippets, an expression containing a constant `1` will be created. 

For more details, see [`Expression.Constant Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.constant?view=net-8.0)

## CH2.4 -- an expression about assigment operator
You can create an expression about assigment operator using `Expression.Assign` static method call.

```
BinaryExpression binaryExpression = Expression.Assign(
                                      Expression.Variable(typeof(int) , "x") ,
                                      Expression.Constant(42)
                                    );
```

## examples
### example 1
Invoking this method

```
        /// <summary>
        /// illustrate assignment operations.  
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();

            binaryExpressions.Add(
                Expression.Assign(
                    Expression.Variable(typeof(int) , "x") ,
                    Expression.Constant(42)
                )
            );

            Console.WriteLine("About `binaryExpressions`");
            binaryExpressions.ForEach(
                (binaryExpression) =>
                {
                    string infoText = binaryExpression.GetInfo();
                    Console.WriteLine(infoText);
                }
            );
            Console.WriteLine();
        }
```

where

`BinaryExpression.GetInfo` extension method is defined in `BinaryExpressionsExtensionMethods` static class.

`BinaryExpressionsExtensionMethods` static class in `ExpressionsExtensionMethods.cs`

```
public static class BinaryExpressionsExtensionMethods
    {
        public static string GetInfo(
            this BinaryExpression binaryExpression
        )
        {
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendFormat("binaryExpression.Left:{0}\n" , binaryExpression.Left);
            stringBuilder.AppendFormat("binaryExpression.Right:{0}\n" , binaryExpression.Right);
            stringBuilder.AppendFormat("binaryExpression.IsLifted:{0}\n" , binaryExpression.IsLifted);
            stringBuilder.AppendFormat("binaryExpression.IsLiftedToNull:{0}\n" , binaryExpression.IsLiftedToNull);
            return stringBuilder.ToString();
        }
    }
```

It will output

```
In TestMethod8 method call,
About `binaryExpressions`
binaryExpression.Left:x
binaryExpression.Right:42
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False


```

## reference
### API docs
+ [`Expression.Variable Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.variable?view=net-8.0)
+ [`Expression.Parameter Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.parameter?view=net-8.0)
+ [`Expression.Constant Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.constant?view=net-8.0)
+ [`Expression.Assign(Expression, Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.assign?view=net-8.0)
