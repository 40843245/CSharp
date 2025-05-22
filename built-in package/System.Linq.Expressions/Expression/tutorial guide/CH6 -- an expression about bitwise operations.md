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
`Expression.AndAssign` static method can do bitwise `AND` operation (without short circuit) to the parameter value and assign it back to the parameter value.

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

When compiling `Expression.Or` and then execute it, it invokes `|` (bitwise `OR` operator). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `OR` operation (without short circuit) to the parameter value.
                Expression.Or(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.OrAssign` static method
`Expression.OrAssign` static method can do bitwise `XOR` operation (without short circuit) to the parameter value and assign it back to the parameter value.

When compiling `Expression.OrAssign` and then execute it, it invokes `|=` (bitwise `OR` operator with assignment). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `OR` operation (without short circuit) to the parameter value.
                Expression.OrAssign(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.ExclusiveOr` static method
`Expression.ExclusiveOr` static method can do bitwise `OR` operation (without short circuit) to the parameter value.

When compiling `Expression.ExclusiveOr` and then execute it, it invokes `^` (bitwise `XOR` operator). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.ExclusiveOr(
                // This expression represents a lambda expression
                // that do bitwise `XOR` operation (without short circuit) to the parameter value.
                Expression.Or(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.ExclusiveOrAssign` static method
`Expression.ExclusiveOrAssign` static method can do bitwise `XOR` operation (without short circuit) to the parameter value and assign it back to the parameter value.

When compiling `Expression.ExclusiveOrAssign` and then execute it, it invokes `^=` (bitwise `XOR` operator with assignment). 

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `XOR` operation (without short circuit) to the parameter value.
                Expression.OrAssign(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );
```

### `Expression.Not` static method
`Expression.Not` static method can do two complement operation (without short circuit) to the parameter value and assign it back to the parameter value.

When compiling `Expression.Not` and then execute it, it invokes `~` (one complement operator) and plus `1` then assign it back.

> [!TIP]
> About digital system knowledgement.
>  
> performing two's complement for unsigned integer is equivalent to performing one's complement for unsigned integer then plus 1 (but you have to fix it when `plus 1` causes overflow).

Think of 

compiling and executing it 

```
int constant = 42;
UnaryExpression unaryExpression = Expression.Not(Expression.Constant(42));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
var result = lambdaExpression.Compile().DynamicInvoke();
```

will get the result

two's complement of `42`

which has same result of

```
int constant = 42;
var result = ~constant + 1 // and fix it when `+1` causes overflows
```

For example,

```
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise two's complementation (without short circuit) to the parameter value.
                Expression.Not(
                    Expression.Constant(42)
                )
            );
```

### `Expression.OnesComplement` static method
`Expression.OnesComplement` static method can do one complement operation (without short circuit) to the parameter value and assign it back to the parameter value.

When compiling `Expression.OnesComplement` and then execute it, it invokes `~=` (one complement operator with assignment). 

Think of 

compiling and executing it 

```
int constant = 42;
UnaryExpression unaryExpression = Expression.OnesComplement(Expression.Constant(constant));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
var result = lambdaExpression.Compile().DynamicInvoke();
```

will get one's complement of `42`

which has same result of

```
int constant = 42;
var result = ~constant;
```

For example,

```
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise one's complementation (without short circuit) to the parameter value.
                Expression.OnesComplement(
                    Expression.Constant(42)
                )
            );
```

### `Expression.LeftShift` static method
`Expression.LeftShift` static method can do left shiftment to the parameter value.

When compiling `Expression.LeftShift` and then execute it, it invokes `<<` (left shift operator). 

Think of 

compiling and executing it 

```
int leftOperand = 4;
int rightOperand = 2;
ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
UnaryExpression unaryExpression = Expression.LeftShift(Expression.Constant(leftOperand));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
LambdaExpression lambdaExpression = Expression.Lambda(
                                      binaryExpression ,
                                      new List<ParameterExpression>() { parameterExpression }
                                    );
var compiledDelegateExpression = lambdaExpression.Compile();
var result = compiledDelegateExpression.DynamicInvoke(rightOperand);
```

will get result of left shifting 2 bits for 4

which has same result of

```
int leftOperand = 4;
int rightOperand = 2;
var result = leftOperand << rightOperand;
```

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do left shiftment to the parameter value.
                Expression.LeftShift(
                    parameterExpression,
                    Expression.Constant(42)
                )
            );
```

### `Expression.LeftShiftAssign` static method
`Expression.LeftShiftAssign` static method can do left shiftment to the parameter value.

When compiling `Expression.LeftShiftAssign` and then execute it, it invokes `<<=` (left shift operator with assignment). 

Think of 

compiling and executing it 

```
int leftOperand = 4;
int rightOperand = 2;
ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
UnaryExpression unaryExpression = Expression.LeftShiftAssign(Expression.Constant(leftOperand));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
LambdaExpression lambdaExpression = Expression.Lambda(
                                      binaryExpression ,
                                      new List<ParameterExpression>() { parameterExpression }
                                    );
var compiledDelegateExpression = lambdaExpression.Compile();
var result = compiledDelegateExpression.DynamicInvoke(rightOperand);
```

will get result of left shifting 2 bits for 4

which has same result of

```
int leftOperand = 4;
int rightOperand = 2;
var result = leftOperand <<= rightOperand;
```

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do left shiftment to the parameter value.
                Expression.LeftShiftAssign(
                    parameterExpression,
                    Expression.Constant(42)
                )
            );
```

### `Expression.RightShift` static method
`Expression.RightShift` static method can do right shiftment to the parameter value.

When compiling `Expression.RightShift` and then execute it, it invokes `>>` (right shift operator). 

Think of 

compiling and executing it 

```
int leftOperand = 4;
int rightOperand = 2;
ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
UnaryExpression unaryExpression = Expression.RightShift(Expression.Constant(leftOperand));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
LambdaExpression lambdaExpression = Expression.Lambda(
                                      binaryExpression ,
                                      new List<ParameterExpression>() { parameterExpression }
                                    );
var compiledDelegateExpression = lambdaExpression.Compile();
var result = compiledDelegateExpression.DynamicInvoke(rightOperand);
```

will get result of right shifting 2 bits for 4

which has same result of

```
int leftOperand = 4;
int rightOperand = 2;
var result = leftOperand >> rightOperand;
```

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do right shiftment to the parameter value.
                Expression.RightShift(
                    parameterExpression,
                    Expression.Constant(42)
                )
            );
```

### `Expression.RightShiftAssign` static method
`Expression.RightShiftAssign` static method can do right shiftment to the parameter value.

When compiling `Expression.RightShiftAssign` and then execute it, it invokes `>>=` (right shift operator with assignment). 

Think of 

compiling and executing it 

```
int leftOperand = 4;
int rightOperand = 2;
ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
UnaryExpression unaryExpression = Expression.RightShiftAssign(Expression.Constant(leftOperand));
LambdaExpression lambdaExpression = Expression.Lambda<Func<int>>(unaryExpression);
LambdaExpression lambdaExpression = Expression.Lambda(
                                      binaryExpression ,
                                      new List<ParameterExpression>() { parameterExpression }
                                    );
var compiledDelegateExpression = lambdaExpression.Compile();
var result = compiledDelegateExpression.DynamicInvoke(rightOperand);
```

will get result of right shifting 2 bits for 4

which has same result of

```
int leftOperand = 4;
int rightOperand = 2;
var result = leftOperand >>= rightOperand;
```

For example,

```
            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();
            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do right shiftment to the parameter value.
                Expression.RightShiftAssign(
                    parameterExpression,
                    Expression.Constant(42)
                )
            );
```

## examples
See examples in CH8.

## reference
### API docs
+ [`Expression.And Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.and?view=net-8.0)
+ [`Expression.AndAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.andassign?view=net-8.0)
+ [`Expression.Or Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.or?view=net-8.0)
+ [`Expression.OrAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.orassign?view=net-8.0)
+ [`Expression.ExclusiveOr Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.exclusiveor?view=net-8.0)
+ [`Expression.ExclusiveOrAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.exclusiveorassign?view=net-8.0)
+ [`Expression.Not Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.not?view=net-8.0)
+ [`Expression.OnesComplement Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.onescomplement?view=net-8.0)
+ [`Expression.LeftShift Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.leftshift?view=net-8.0)
+ [`Expression.LeftShiftAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.leftshiftassign?view=net-8.0)
+ [`Expression.RightShift Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.rightshift?view=net-8.0)
+ [`Expression.RightShiftAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.rightshiftassign?view=net-8.0)
