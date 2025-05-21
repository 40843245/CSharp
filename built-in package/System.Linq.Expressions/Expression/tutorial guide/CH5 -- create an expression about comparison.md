# CH5 -- create an expression about comparison
## objectives
You will learn how to

+ create an expression that compares two numbers

## CH5.1 -- create an expression that compares two numbers
### `Expression.Equal` static method
`Expression.Equal` static method can compares two expression that containing a number (which is a kind of a constant) are equal and then return an expression -- `BinaryExpression` type.

For example,

```
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            
            BinaryExpression equalExpression = Expression.Equal(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );
            Console.WriteLine("Is {0} equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(equalExpression).Compile()());
```

### `Expression.NotEqual` static method
`Expression.NotEqual` static method can compares two expression that containing a number (which is a kind of a constant) are not equal and then return an expression -- `BinaryExpression` type.

For example,

```
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            BinaryExpression notEqualExpression = Expression.NotEqual(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );
            Console.WriteLine("Is {0} not equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(notEqualExpression).Compile()());
```

### `Expression.LessThan` static method
`Expression.LessThan` static method can compares two expression that containing a number (which is a kind of a constant) whether left value (of expression) is less than right value (of that) and then return an expression -- `BinaryExpression` type.

For example,

```
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            BinaryExpression notEqualExpression = Expression.LessThan(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );
            Console.WriteLine("Is {0} less than {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(lessThanExpression).Compile()());
```

### `Expression.LessThanOrEqual` static method
`Expression.LessThanOrEqual` static method can compares two expression that containing a number (which is a kind of a constant) whether left value (of expression) is less than or equal to right value (of that) and then return an expression -- `BinaryExpression` type.

For example,

```
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            BinaryExpression notEqualExpression = Expression.LessThanOrEqual(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );
            Console.WriteLine("Is {0} less than or equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>
```

### `Expression.GreaterThan` static method
`Expression.GreaterThan` static method can compares two expression that containing a number (which is a kind of a constant) whether left value (of expression) is greater than right value (of that) and then return an expression -- `BinaryExpression` type.

For example,

```
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            BinaryExpression notEqualExpression = Expression.GreaterThan(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );
            Console.WriteLine("Is {0} greater than {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(greaterThanExpression).Compile()());
```

### `Expression.GreaterThanOrEqual` static method
`Expression.GreaterThanOrEqual` static method can compares two expression that containing a number (which is a kind of a constant) whether left value (of expression) is greater than or equal to right value (of that) and then return an expression -- `BinaryExpression` type.

For example,

```
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            BinaryExpression notEqualExpression = Expression.GreaterThanOrEqual(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );
            Console.WriteLine("Is {0} greater than or equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(greaterThanExpression).Compile()());
```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to compare two expression with data type `System.Int32`.
        /// </summary>
        public static void TestMethod13()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ExpressionsOutput.CompareExpressionAndOutput(40,40);
            ExpressionsOutput.CompareExpressionAndOutput(45,42);
            ExpressionsOutput.CompareExpressionAndOutput(42,45);
        }
```

where 

`CompareExpressionAndOutput` static helper method is defined in static class `ExpressionsOutput`

`ExpressionsOutput.cs`

```
    public static class ExpressionsOutput
    {
        public static void CompareExpressionAndOutput(
            int leftValue,
            int rightValue
        )
        {
            Expression expressionConstantLeftValue = Expression.Constant(leftValue);
            Expression expressionConstantRightValue = Expression.Constant(rightValue);
            
            BinaryExpression equalExpression = Expression.Equal(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );

            BinaryExpression notEqualExpression = Expression.NotEqual(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );

            BinaryExpression lessThanExpression = Expression.LessThan(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );

            BinaryExpression lessThanOrEqualToExpression = Expression.LessThanOrEqual(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );

            BinaryExpression greaterThanExpression = Expression.GreaterThan(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );


            BinaryExpression greaterThanOrEqualToExpression = Expression.GreaterThanOrEqual(
                expressionConstantLeftValue ,
                expressionConstantRightValue
            );

            Console.WriteLine("Let's compare {0} and {1}",leftValue,rightValue);
            // The following statement first creates an expression tree,
            // then compiles it, and then executes it.
            Console.WriteLine("Is {0} equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(equalExpression).Compile()());

            Console.WriteLine("Is {0} not equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(notEqualExpression).Compile()());

            Console.WriteLine("Is {0} less than {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(lessThanExpression).Compile()());

            Console.WriteLine("Is {0} less than or equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(lessThanOrEqualToExpression).Compile()());

            Console.WriteLine("Is {0} greater than {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(greaterThanExpression).Compile()());

            Console.WriteLine("Is {0} greater than or equal to {1}? {2}" , leftValue , rightValue , Expression.Lambda<Func<bool>>(greaterThanExpression).Compile()());

            Console.WriteLine();
        }
    }
```

It will output following

```
In TestMethod13 method call,
Let's compare 40 and 40
Is 40 equal to 40? True
Is 40 not equal to 40? False
Is 40 less than 40? False
Is 40 less than or equal to 40? True
Is 40 greater than 40? False
Is 40 greater than or equal to 40? False

Let's compare 45 and 42
Is 45 equal to 42? False
Is 45 not equal to 42? True
Is 45 less than 42? False
Is 45 less than or equal to 42? False
Is 45 greater than 42? True
Is 45 greater than or equal to 42? True

Let's compare 42 and 45
Is 42 equal to 45? False
Is 42 not equal to 45? True
Is 42 less than 45? True
Is 42 less than or equal to 45? True
Is 42 greater than 45? False
Is 42 greater than or equal to 45? False

```

#### demo project

## reference
### API docs
+ [`Expression.Equal Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.equal?view=net-8.0)
+ [`Expression.NotEqual Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.notequal?view=net-8.0)
+ [`Expression.LessThan Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.lessthan?view=net-8.0)
+ [`Expression.LessThanOrEqual Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.lessthanorequal?view=net-8.0)
+ [`Expression.GreaterThan Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.greaterthan?view=net-8.0)
+ [`Expression.GreaterThanOrEqual Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.greaterthanorequal?view=net-8.0)
