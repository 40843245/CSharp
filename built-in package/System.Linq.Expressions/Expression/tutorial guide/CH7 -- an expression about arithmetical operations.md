# CH7 -- an expression about arithmetical operations
## objectives
You will learn

+ an expression about arithmetical operations

## CH7.1 -- an expression about arithmetical operations
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

### `Expression.Modulo` static method
`Expression.Modulo` static method can arithmetically modulus from left number to right numbers, which take the remainder after integer division from left number to right number.

When an expression created by `Expression.Modulo` static method is compiled then executed, it will execute `%` operation (here is, binary modulus operation).

For example,

```
                ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "arg");
                // This expression represents a lambda expression
                // that multiply by 5 to the parameter value.
                BinaryExpression binaryExpression = Expression.Modulo(
                        intParameterExpression1 ,
                        Expression.Constant(5)
                );
```

### `Expression.Power` static method
`Expression.Power` static method can arithmetically power left number to right numbers, which raises left number by right number.

When an expression created by `Expression.Power` static method is compiled then executed, it will execute `Math.Pow` in C#.

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


## reference
### API docs
+ [``]()
+ [``]()
+ [``]()
+ [``]()
+ [``]()
+ [``]()
+ [``]()
+ [``]()
+ [``]()
+ [``]()
