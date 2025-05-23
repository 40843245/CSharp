# CH9 -- an expression about arithmetical operations
## objectives
You will learn

+ an expression about arithmetical operations

## CH9.1 -- an expression about arithmetical operations
### `Expression.Negate` static method
`Expression.Negate` static method can arithmetically negate an number.

When an expression created by `Expression.Negate` static method is compiled then executed, it will execute `-` operation (here is, unary minus which means `negate` the number).

You can think it of

```
int constant = 42;
UnaryExpression unaryExpression = Expression.Negate(Expression.Constant(constant));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
var result = lambdaExpression.Compile().DynamicInvoke();
```

will negate the number `42` 

which has same result of

```
int constant = 42;
var result = -constant;
```

For example,

```
                int constant = 42;
                List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
                unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do arithmetic negation operation (without short circuit) to the parameter value.
                Expression.Negate(
                    Expression.Constant(constant)
                );
```

### `Expression.Add` static method
`Expression.Add` static method can arithmetically add two numbers.

When an expression created by `Expression.Add` static method is compiled then executed, it will execute `+` operation (here is, binary plus operation which means `adds` two numbers).

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that add 1 to the parameter value.
                BinaryExpression binaryExpression = Expression.Add(
                        intParameterExpression1 ,
                        Expression.Constant(1)
                );
```

### `Expression.AddAssign` static method
`Expression.AddAssign` static method can arithmetically add two numbers.

When an expression created by `Expression.AddAssign` static method is compiled then executed, it will execute `+` operation (here is, binary plus operation which means `adds` two numbers) then assign it back.

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that add 1 to the parameter value.
                BinaryExpression binaryExpression = Expression.AddAssign(
                        intParameterExpression1 ,
                        Expression.Constant(1)
                );
```

### `Expression.Subtract` static method
`Expression.Subtract` static method can arithmetically subtract from left number to right number.

When an expression created by `Expression.Subtract` static method is compiled then executed, it will execute `-` operation (here is, binary minus operation which means `subtract` two numbers).

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that subtract 1 to the parameter value.
                BinaryExpression binaryExpression = Expression.Subtract(
                        intParameterExpression1 ,
                        Expression.Constant(1)
                );
```

### `Expression.SubtractAssign` static method
`Expression.SubtractAssign` static method can arithmetically subtract from left number to right number.

When an expression created by `Expression.SubtractAssign` static method is compiled then executed, it will execute `-` operation (here is, binary minus operation which means `subtract` two numbers) and assign it back.

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that subtract 1 to the parameter value.
                BinaryExpression binaryExpression = Expression.SubtractAssign(
                        intParameterExpression1 ,
                        Expression.Constant(1)
                );
```

### `Expression.Multiply` static method
`Expression.Multiply` static method can arithmetically left number times right numbers.

When an expression created by `Expression.Multiply` static method is compiled then executed, it will execute `*` operation (here is, binary multiplication operation which means `multiply` two numbers).

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that multiply by 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.Multiply(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.MultiplyAssign` static method
`Expression.MultiplyAssign` static method can arithmetically left number times right numbers.

When an expression created by `Expression.MultiplyAssign` static method is compiled then executed, it will execute `*` operation (here is, binary multiplication operation which means `multiply` two numbers) and assign it back.

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that multiply by 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.MultiplyAssign(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.Divide` static method
`Expression.Divide` static method can arithmetically divide from left number by right numbers then round down the resultant to first digit of floating-point part.

When an expression created by `Expression.Divide` static method is compiled then executed, it will execute `/` operation (here is, binary integer division operation).
For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that multiply by 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.Divide(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.DivideAssign` static method
`Expression.DivideAssign` static method can arithmetically divide from left number by right numbers then round down the resultant to first digit of floating-point part.

When an expression created by `Expression.DivideAssign` static method is compiled then executed, it will execute `/` operation (here is, binary integer division operation) and assign it back.

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that multiply by 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.DivideAssign(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.Modulo` static method
`Expression.Modulo` static method can arithmetically modulus from left number to right numbers, which take the remainder after integer division from left number to right number.

When an expression created by `Expression.Modulo` static method is compiled then executed, it will execute `%` operation (here is, binary modulus operation).

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that modulus 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.Modulo(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.ModuloAssign` static method
`Expression.ModuloAssign` static method can arithmetically modulus from left number to right numbers, which take the remainder after integer division from left number to right number.

When an expression created by `Expression.ModuloAssign` static method is compiled then executed, it will execute `%` operation (here is, binary modulus operation), and assign it back.

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that modulus 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.ModuloAssign(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.Power` static method
`Expression.Power` static method can arithmetically power left number to right numbers, which raises left number by right number.

When an expression created by `Expression.Power` static method is compiled then executed, it will execute `Math.Pow` in `C#`.

> [!CAUTION]
> Watch out the type in the expressions.
>
> According to API docs [`Math.Pow(Double, Double) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.math.pow?view=net-9.0),
>
> `Math.Pow` static method only accepts two arguments with double type and then it returns double type.
>
> So, if you either pass left number without double type (in the expression) or right nunber without double type (in the expression),
>
> it will throw `System.Exception.ArgumentException` exception at runtime.

For example,

```
                ParameterExpression doubleParameterExpression2 = Expression.Parameter(typeof(double) , "arg");
                // This expression represents a lambda expression
                // that power of 4 to the parameter value.
                BinaryExpression binaryExpression = Expression.Power(
                    doubleParameterExpression1 ,
                    Expression.Constant(4d)
                 );
```

### `Expression.PowerAssign` static method
`Expression.PowerAssign` static method can arithmetically power left number to right numbers, which raises left number by right number. Then assign it back.

When an expression created by `Expression.Power` static method is compiled then executed, it will execute `Math.Pow` in `C#` then assign it back.

> [!CAUTION]
> Watch out the type in the expressions.
>
> According to API docs [`Math.Pow(Double, Double) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.math.pow?view=net-9.0),
>
> `Math.Pow` static method only accepts two arguments with double type and then it returns double type.
>
> So, if you either pass left number without double type (in the expression) or right nunber without double type (in the expression),
>
> it will throw `System.Exception.ArgumentException` exception at runtime.

For example,

```
                ParameterExpression doubleParameterExpression2 = Expression.Parameter(typeof(double) , "arg");
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                UnaryExpression doubleParameterExpression1 = Expression.Convert(intParameterExpression1 , typeof(double));
                // This expression represents a lambda expression
                // that power of 4 to the parameter value.
                BlockExpression blockExpression = Expression.Block(
                                                    new [ ] { doubleVariable1 } , // Declare doubleVariable1 as a local variable
                                                    Expression.Assign(doubleVariable1 , doubleParameterExpression1) , // Initialize it
                                                    Expression.PowerAssign(doubleVariable1 , Expression.Constant(4d)) , // Perform the operation
                                                    doubleVariable1 // The last expression in the block is its return value
                                                  );
```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate mathematically operations.  
        /// </summary>
        public static void TestMethod9()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
            UnaryExpression doubleParameterExpression1 = Expression.Convert(intParameterExpression1 , typeof(double));

            ParameterExpression doubleParameterExpression2 = Expression.Parameter(typeof(double) , "arg");

            ParameterExpression intVariable1 = Expression.Variable(typeof(int) , "returnValue");
            ParameterExpression doubleVariable1 = Expression.Variable(typeof(double) , "returnValue");

            List<Expression> expressionsToTest = new List<Expression>();
            
            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that add 1 to the parameter value.
                Expression.Add(
                    intParameterExpression1 ,
                    Expression.Constant(1)
                )
           );

            expressionsToTest.Add(
               // This expression represents a lambda expression
               // that add 1 to the parameter value.
               Expression.AddAssign(
                   intParameterExpression1 ,
                   Expression.Constant(1)
               )
          );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that subtract 1 to the parameter value.
                Expression.Subtract(
                    intParameterExpression1 ,
                    Expression.Constant(1)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that subtract 1 to the parameter value.
                Expression.SubtractAssign(
                    intParameterExpression1 ,
                    Expression.Constant(1)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that mutliply by 2 to the parameter value.
                Expression.Multiply(
                    intParameterExpression1 ,
                    Expression.Constant(2)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that mutliply by 2 to the parameter value.
                Expression.MultiplyAssign(
                    intParameterExpression1 ,
                    Expression.Constant(2)
                )
           );


            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that divided by 5 to the parameter value.
                Expression.Divide(
                    intParameterExpression1 ,
                    Expression.Constant(5)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that divided by 5 to the parameter value.
                Expression.DivideAssign(
                    intParameterExpression1 ,
                    Expression.Constant(5)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that modulo 4 to the parameter value.
                Expression.Modulo(
                    intParameterExpression1 ,
                    Expression.Constant(4)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that modulo 4 to the parameter value.
                Expression.ModuloAssign(
                    intParameterExpression1 ,
                    Expression.Constant(4)
                )
           );

            expressionsToTest.Add(
                // This expression represents a lambda expression
                // that power of 4 to the parameter value.
                Expression.Power(
                    doubleParameterExpression1 ,
                    Expression.Constant(4d)
                )
           );

            expressionsToTest.Add(
               // This expression represents a lambda expression
               // that power of 4 to the parameter value.
               Expression.Block(
                    new [ ] { doubleVariable1 } , // Declare doubleVariable1 as a local variable
                    Expression.Assign(doubleVariable1 , doubleParameterExpression1) , // Initialize it
                    Expression.PowerAssign(doubleVariable1 , Expression.Constant(4d)) , // Perform the operation
                    doubleVariable1 // The last expression in the block is its return value
                )
           );

            expressionsToTest.ForEach(
                (expressionToTest) =>
                {
                    LambdaExpression lambdaExpression = Expression.Lambda(
                        expressionToTest ,
                        new List<ParameterExpression>() { intParameterExpression1 }
                    );
                    var delegateFunction = lambdaExpression.Compile();

                    Console.WriteLine("delegateFunction.DynamicInvoke({0}):{1}" , 2 , delegateFunction.DynamicInvoke(2));
                }
            );
        }
```

will output following 

```
In TestMethod9 method call,
delegateFunction.DynamicInvoke(2):3
delegateFunction.DynamicInvoke(2):3
delegateFunction.DynamicInvoke(2):1
delegateFunction.DynamicInvoke(2):1
delegateFunction.DynamicInvoke(2):4
delegateFunction.DynamicInvoke(2):4
delegateFunction.DynamicInvoke(2):0
delegateFunction.DynamicInvoke(2):0
delegateFunction.DynamicInvoke(2):2
delegateFunction.DynamicInvoke(2):2
delegateFunction.DynamicInvoke(2):16
delegateFunction.DynamicInvoke(2):16
```


## reference
### API docs
+ [`Expression.Negate Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.negate?view=net-8.0)
+ [`Expression.Add Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.add?view=net-8.0)
+ [`Expression.AddAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.addassign?view=net-8.0)
+ [`Expression.Subtract Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.subtract?view=net-8.0)
+ [`Expression.SubtractAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.subtractassign?view=net-8.0)
+ [`Expression.Multiply Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.multiply?view=net-8.0)
+ [`Expression.MultiplyAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.multiplyassign?view=net-8.0)
+ [`Expression.Divide Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.divide?view=net-8.0)
+ [`Expression.DivideAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.divideassign?view=net-8.0)
+ [`Expression.Modulo Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.modulo?view=net-8.0)
+ [`Expression.ModuloAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.moduloassign?view=net-8.0)
+ [`Expression.Power Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.power?view=net-8.0)
+ [`Expression.PowerAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.powerassign?view=net-8.0)
