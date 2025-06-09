# CH3 -- create a lambda expression
## objectives
You will learn how to

+ create a lambda expression

## CH3.1 -- create a lambda expression
You can create a a lambda expression using `Expression.Lambda` static method call.

```
LambdaExpression lambdaExpression = Expression.Lambda(
  expression, // its type MUST be `System.Linq.Expression.Expression` or its derived class
  parameters // its type MUST be `System.Collections.Generic.IEnumerable<System.Linq.Expressions.ParameterExpression>?` or its derived class, it can be nullable
);
```

For more overloads, see [`Expression.Lambda Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.lambda?view=net-8.0)

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");

            // This expression represents a lambda expression
            // that adds 1 to the parameter value.
            LambdaExpression lambdaExpression = Expression.Lambda(
                Expression.Add(
                    parameterExpression ,
                    Expression.Constant(1)
                ) ,
                new List<ParameterExpression>() { parameterExpression }
            );
```

## reference
### API docs
+ [`Expression.Lambda Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.lambda?view=net-8.0)
