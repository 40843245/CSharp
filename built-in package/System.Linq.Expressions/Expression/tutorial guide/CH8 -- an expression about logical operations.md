# CH8 -- an expression about logical operations
## objectives
You will learn

+ an expression about logical operations

## CH8.1 -- an expression about logical operations
### `Expression.AndAlso` static method
`Expression.AndAlso` static method creates an expression that evaluate two conditions by logical `AND` operation (with short-ciruit feature).

When `Expression.AndAlso` static method is created, compiled and executed, it will execute `&&` (logical `AND` operation).

For example,

```
            MethodInfo isNullOrEmptyMethod = typeof(string).GetMethod("IsNullOrEmpty" , new [ ] { typeof(string) });
            MethodInfo startsWithMethod = typeof(string).GetMethod("StartsWith" , new [ ] { typeof(string) });

            Expression startWithCatCondition = Expression.AndAlso(
                Expression.Not(Expression.Call(isNullOrEmptyMethod , textParam)) , 
                Expression.Call(textParam , startsWithMethod , Expression.Constant("Cat"))
            );
```

### `Expression.OrElse` static method
`Expression.OrElse` static method creates an expression that evaluate two conditions by logical `OR` operation (with short-ciruit feature).

When `Expression.OrElse` static method is created, compiled and executed, it will execute `||` (logical `OR` operation).

For example,

```
                ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");
    
                ParameterExpression iParam = Expression.Parameter(typeof(int) , "i");
                ParameterExpression jParam = Expression.Parameter(typeof(int) , "j");
                ParameterExpression xParam = Expression.Parameter(typeof(int) , "x");
    
                MethodInfo writeLineMethod = typeof(Console).GetMethod("WriteLine" , new [ ] { typeof(string) });
                MethodInfo stringFormatMethod = typeof(string).GetMethod("Format" , new [ ] { typeof(string) , typeof(object) });
    
                Expression<Func<int , bool>> lambdaExpression1 = Expression.Lambda<Func<int , bool>>(
                        Expression.Block(
                            // Expression for Console.WriteLine($"i:{i}")
                            Expression.Call(
                                writeLineMethod ,
                                Expression.Call(
                                    stringFormatMethod ,
                                    Expression.Constant("i:{0}") ,
                                    Expression.Convert(iParam , typeof(object)) // Convert int to object for string.Format
                                )
                            ) ,
                            // Expression for return i > 0; (the result of the block)
                            Expression.GreaterThan(iParam , Expression.Constant(0))
                        ),
                        iParam
                );

                // Prepare for Parameter Rebinding
                // We need to tell our rebinder to replace 'iParam' and 'jParam'
                // with the new 'xParam' in their respective bodies.    
                Expression<Func<int , bool>> lambdaExpression2 = Expression.Lambda<Func<int , bool>>(
                        Expression.Block(
                            // Expression for Console.WriteLine($"j:{j}")
                            Expression.Call(
                                writeLineMethod ,
                                Expression.Call(
                                    stringFormatMethod ,
                                    Expression.Constant("j:{0}") ,
                                    Expression.Convert(jParam , typeof(object)) // Convert int to object for string.Format
                                )
                            ) ,
                            // Expression for return j <= 0; (the result of the block)
                            Expression.LessThanOrEqual(jParam , Expression.Constant(0))
                        ) ,
                        jParam
                );

                var parameterMap = new Dictionary<ParameterExpression , ParameterExpression>
                {
                    { lambdaExpression1.Parameters[0], xParam }, // Map iParam to xParam
                    { lambdaExpression2.Parameters[0], xParam }  // Map jParam to xParam
                };

                // Parameter Rebinding
                var parameterRebinder = new ParameterRebinder(parameterMap);
    
                // Rebind the Bodies
                Expression body1Rebound = parameterRebinder.Visit(lambdaExpression1.Body);
                Expression body2Rebound = parameterRebinder.Visit(lambdaExpression2.Body);

                // This expression represents a lambda expression
                // that do logical `OR` operation (with short circuit) to the parameter value.
                LambdaExpression lambdaExpression = Expression.Lambda<Func<int , bool>>(
                                                       Expression.OrElse(
                                                          body1Rebound ,
                                                          body2Rebound
                                                       ) ,
                                                       xParam // The new lambda's single parameter
                                                    );
```
