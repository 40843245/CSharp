# CH23 -- create an expression with given specific type
## objectives
You will learn how to

+ create an binary expression `BinaryExpression` with given `ExpressionType`
+ create an unary expression `UnaryExpression` with given `ExpressionType`

## CH23.1 -- create an binary expression `BinaryExpression` with given `ExpressionType`
### `Expression.MakeBinary` static method
Given specific type -- `ExpressionType` enum value, one can create an binary expression `BinaryExpression` by invoking `Expression.MakeBinary` static method.

> [!CAUTION]
> Watch out the specifice type -- `ExpressionType` enum value.
> 
> It MUST be an binary operation.
>
> Otherwise, it might throw exception at runtime.

Basic structure:

```
                Expression.MakeBinary(
                    binaryType, // It MUST be `ExpressionType` enum value and an binary expression, such as `ExpressionType.Divide`
                    leftValueExpression, // It MUST be an expression containing a constant (such as `Expression.Constant(leftValue)`) , or a variable (such as `Expression.Variable(typeof(int) , "x")`)
                    rightValueExpression // same as above
                )
```

## CH23.2 -- create an unary expression `UnaryExpression` with given `ExpressionType`
### `Expression.MakeUnary` static method
Given specific type -- `ExpressionType` enum value, one can create an unary expression `UnaryExpression` by invoking `Expression.MakeUnary` static method.

> [!CAUTION]
> Watch out the specifice type -- `ExpressionType` enum value.
> 
> It MUST be an unary operation.
>
> Otherwise, it might throw exception at runtime.

Basic structure:

```
                Expression.MakeUnary(
                    unaryType, // It MUST be `ExpressionType` enum value and an unary expression, such as `ExpressionType.Divide`
                    valueExpression, // It MUST be an expression containing a constant (such as `Expression.Constant(leftValue)`) , or a variable (such as `Expression.Variable(typeof(int) , "x")`)
                    type // It MUST be type `Type` that specifies the type to be converted to (pass `null` if not applicable).
                )
```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.MakeBinary`.
        /// </summary>
        public static void TestMethod38()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();

            binaryExpressions.Add(
                // Create a BinaryExpression that represents adding 14 from 53.
                Expression.MakeBinary(
                    ExpressionType.Add ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents subtracting 14 from 53.
                Expression.MakeBinary(
                    ExpressionType.Subtract ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents 53 times 14.
                Expression.MakeBinary(
                    ExpressionType.Multiply ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents dividing 53 by 14.
                Expression.MakeBinary(
                    ExpressionType.Divide ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents modulus 53 by 14.
                Expression.MakeBinary(
                    ExpressionType.Modulo ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents raise 53 by 14.
                Expression.MakeBinary(
                    ExpressionType.Power ,
                    Expression.Constant(53d) ,
                    Expression.Constant(14d)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents whether 53 equal to 14 or note.
                Expression.MakeBinary(
                    ExpressionType.Equal ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents whether 53 not equal to 14 or not.
                Expression.MakeBinary(
                    ExpressionType.NotEqual ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents whether 53 greater than 14 or not.
                Expression.MakeBinary(
                    ExpressionType.GreaterThan ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents whether 53 greater than or equal to 14 or not.
                Expression.MakeBinary(
                    ExpressionType.GreaterThanOrEqual ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents whether 53 less than 14 or not.
                Expression.MakeBinary(
                    ExpressionType.LessThan ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.Add(
                // Create a BinaryExpression that represents whether 53 less than or equal to 14 or not.
                Expression.MakeBinary(
                    ExpressionType.LessThanOrEqual ,
                    Expression.Constant(53) ,
                    Expression.Constant(14)
                )
            );

            binaryExpressions.ForEach(
                binaryExpression => Console.WriteLine(binaryExpression.ToString())
            );
        }
```

will output following

```
In TestMethod38 method call,
(53 + 14)
(53 - 14)
(53 * 14)
(53 / 14)
(53 % 14)
(53 ^ 14)
(53 == 14)
(53 != 14)
(53 > 14)
(53 >= 14)
(53 < 14)
(53 <= 14)
```

### example 2
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.MakeUnary`.
        /// </summary>
        public static void TestMethod39()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ParameterExpression x = Expression.Variable(typeof(int) , "x"); // int x;
          
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();

            unaryExpressions.Add(
                // Create a UnaryExpression that represents 53 + 1
                Expression.MakeUnary(
                    ExpressionType.Increment,
                    Expression.Constant(53),
                    null
                )
            );

            unaryExpressions.Add(
                // Create a UnaryExpression that represents 53 - 1
                Expression.MakeUnary(
                    ExpressionType.Decrement ,
                    Expression.Constant(53) ,
                    null
                )
            );

            unaryExpressions.Add(
                // Create a UnaryExpression that represents ++x
                Expression.MakeUnary(
                    ExpressionType.PreIncrementAssign ,
                    x,
                    null
                )
            );

            unaryExpressions.Add(
                // Create a UnaryExpression that represents x++
                Expression.MakeUnary(
                    ExpressionType.PostIncrementAssign ,
                    x ,
                    null
                )
            );

            unaryExpressions.Add(
                // Create a UnaryExpression that represents --x
                Expression.MakeUnary(
                    ExpressionType.PreDecrementAssign ,
                    x ,
                    null
                )
            );

            unaryExpressions.Add(
                // Create a UnaryExpression that represents x--
                Expression.MakeUnary(
                    ExpressionType.PostDecrementAssign ,
                    x ,
                    null
                )
            );

            unaryExpressions.ForEach(
                unaryExpression => Console.WriteLine(unaryExpression.ToString())
            );
        }
```

will output following

```
In TestMethod39 method call,
Increment(53)
Decrement(53)
++x
PostIncrementAssign(x++
--x
PostDecrementAssign(x--
```

## reference
### API docs
+ [`Expression.MakeBinary Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.makebinary?view=net-8.0)
+ [`Expression.MakeUnary Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.makeunary?view=net-8.0)