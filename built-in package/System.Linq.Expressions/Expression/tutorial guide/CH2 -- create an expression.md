# CH2 -- create an expression
## objectives
You will learn how to

+ create an expression containing a variable
+ create an expression containing a parameter
+ create an expression containing a constant

## CH2.1 -- create an expression containing a variable
### `Expression.Variable` static method call
You can create an expression contnaining a variable using `Expression.Variable` static method call.

```
ParameterExpression intParameterExpression1 = Expression.Variable(typeof(int) , "x");
```

In above code snippets, an expression containing a variable will be created. 

The variable name is `x` with int type.

For more details, see [`Expression.Variable Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.variable?view=net-8.0)

## CH2.2 -- create an expression containing a parameter
### `Expression.Parameter` static method call
You can create an expression contnaining a variable using `Expression.Parameter` static method call.

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
```

In above code snippets, an expression containing a parameter will be created. 

The parameter name is `x` with int type.

For more details, see [`Expression.Parameter Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.parameter?view=net-8.0)

## CH2.3 -- create an expression containing a constant
### `Expression.Constant` static method call
You can create an expression contnaining a constant using `Expression.Constant` static method call.

```
Expression expressionConstantOne = Expression.Constant(1);
```

In above code snippets, an expression containing a constant `1` will be created. 

For more details, see [`Expression.Constant Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.constant?view=net-8.0)
