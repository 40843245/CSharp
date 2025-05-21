# CH6 -- an expression about bitwise operations
## objectives
You will learn 

+ an expression about bitwise operations

## CH6.1 -- an expression about bitwise operations
### `Expression.And` static method
`Expression.And` static method can do bitwise `AND` operation (without short circuit) to the parameter value.

When compiling `Expression.And` and then execute it, it invokes `&` (bitwise `AND` operator). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `AND` operation (without short circuit) to the parameter value.
                Expression.And(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.AndAssign` static method
`Expression.And` static method can do bitwise `AND` operation (without short circuit) to the parameter value and assign it back to the parameter value.

When compiling `Expression.And` and then execute it, it invokes `&=` (bitwise `AND` operator with assignment). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `AND` operation (without short circuit) to the parameter value.
                Expression.AndAssign(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.Or` static method
`Expression.And` static method can do bitwise `OR` operation (without short circuit) to the parameter value.

When compiling `Expression.And` and then execute it, it invokes `|` (bitwise `OR` operator). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `AND` operation (without short circuit) to the parameter value.
                Expression.And(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.AndAssign` static method
`Expression.And` static method can do bitwise `AND` operation (without short circuit) to the parameter value and assign it back to the parameter value.

When compiling `Expression.And` and then execute it, it invokes `&=` (bitwise operator with assignment). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `AND` operation (without short circuit) to the parameter value.
                Expression.AndAssign(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```
