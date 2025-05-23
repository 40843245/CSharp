# CH14 -- convert an expression containing a constant from one type to other type
## objectives
You will learn how to

+ convert an expression containing a constant from one type to other type

## CH14.1 -- convert an expression containing a constant from one type to other type
### `Expression.Convert` static method
To convert an expression containing a constant from one type to other type, just simply invoke `Expression.Convert` static method.

> [!CAUTION]
> Watch out the return type of `Expression.Convert` static method.
>
> The return type of `Expression.Convert` static method is `UnaryExpression`, **NOT** `ParameterExpression`. 

For example,

It will convert an expression with parameter named `arg` from `int` type to `double` type.

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
UnaryExpression doubleParameterExpression1 = Expression.Convert(intParameterExpression1 , typeof(double));
```

## examples
### exmaple 1
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
