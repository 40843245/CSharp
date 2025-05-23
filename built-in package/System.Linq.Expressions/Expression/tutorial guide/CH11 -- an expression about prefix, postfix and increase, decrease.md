# CH9 -- an expression about prefix, postfix and increase, decrease
## objectives
You will know 

+ an expression about prefix increase
+ an expression about postfix increase
+ an expression about prefix decrease
+ an expression about postfix decrease

## CH9.1 -- an expression about prefix increase
### `Expression.PreIncrementAssign` static method
`Expression.PreIncrementAssign` static method can create an expression about prefix increase

When the expression is compiled and executed, it will execute the prefix increase operator (`++x`).

For example,

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
UnaryExpression unaryExpression = Expression.PreIncrementAssign(intParameterExpression1);
```

## CH9.2 -- an expression about postfix increase
### `Expression.PostIncrementAssign` static method
`Expression.PostIncrementAssign` static method can create an expression about postfix increase

When the expression is compiled and executed, it will execute the postfix increase operator (`x++`).

For example,

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
UnaryExpression unaryExpression = Expression.PostIncrementAssign(intParameterExpression1);
```

## CH9.3 -- an expression about prefix decrease
### `Expression.PreDecrementAssign` static method
`Expression.PreDecrementAssign` static method can create an expression about prefix decrease

When the expression is compiled and executed, it will execute the prefix decrease operator (`--x`).

For example,

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
UnaryExpression unaryExpression = Expression.PreDecrementAssign(intParameterExpression1);
```

## CH9.4 -- an expression about postfix decrease
### `Expression.PostDecrementAssign` static method
`Expression.PostDecrementAssign` static method can create an expression about postfix decrease

When the expression is compiled and executed, it will execute the postfix decrease operator (`x--`).

For example,

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
UnaryExpression unaryExpression = Expression.PostDecrementAssign(intParameterExpression1);
```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate prefix/postfix increment/decrement operation.
        /// </summary>
        public static void TestMethod12()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");

            List<UnaryExpression> unaryExpressions = new List<UnaryExpression>();

            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do prefix increment to the parameter value.
                Expression.PreIncrementAssign(
                    intParameterExpression1
                )
            );

            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do postfix increment to the parameter value.
                Expression.PostIncrementAssign(
                    intParameterExpression1
                )
            );

            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do prefix decrement to the parameter value.
                Expression.PreDecrementAssign(
                    intParameterExpression1
                )
            );

            unaryExpressions.Add(
                // This expression represents a lambda expression
                // that do postfix decrement to the parameter value.
                Expression.PostDecrementAssign(
                    intParameterExpression1
                )
            );

            int counter = 0;
            Console.WriteLine("About `unaryExpressions`");
            unaryExpressions.ForEach(
                (unaryExpression) =>
                {
                    int constants = 10;
                    var lambdaExpression = Expression.Lambda<Func<int , int>>(unaryExpression , intParameterExpression1);
                    Console.WriteLine("---- In {0}th round ----",counter);
                    string infoText = unaryExpression.GetInfo();
                    Console.WriteLine(infoText);
                    Console.WriteLine();
                    Console.WriteLine("Compile the expression:`{0}`, then execute it with argument value `{1}` will gets `{2}`.", lambdaExpression.ToString(),constants,lambdaExpression.Compile()(constants));
                    Console.WriteLine("---- End of {0}th round ----" , counter);
                    counter++;
                }
            );
            Console.WriteLine();
        }
```

will output following

```
In TestMethod12 method call,
About `unaryExpressions`
---- In 0th round ----
unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:x


Compile the expression:`x => ++x`, then execute it with argument value `10` will gets `11`.
---- End of 0th round ----
---- In 1th round ----
unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:x


Compile the expression:`x => PostIncrementAssign(x++`, then execute it with argument value `10` will gets `10`.
---- End of 1th round ----
---- In 2th round ----
unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:x


Compile the expression:`x => --x`, then execute it with argument value `10` will gets `9`.
---- End of 2th round ----
---- In 3th round ----
unaryExpression.IsLifted:False
unaryExpression.IsLiftedToNull:False
unaryExpression.Operand:x


Compile the expression:`x => PostDecrementAssign(x--`, then execute it with argument value `10` will gets `10`.
---- End of 3th round ----

```

## reference
### API docs
+ [`Expression.PreIncrementAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.preincrementassign?view=net-8.0)
+ [`Expression.PreDecrementAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.predecrementassign?view=net-8.0)
+ [`Expression.PostIncrementAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.postincrementassign?view=net-8.0)
+ [`Expression.PostDecrementAssign Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.postdecrementassign?view=net-8.0)
