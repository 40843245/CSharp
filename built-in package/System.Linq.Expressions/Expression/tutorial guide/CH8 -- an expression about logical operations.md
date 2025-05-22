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

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to use BinaryExpression to do bitwise or logical operations.  
        /// </summary>
        public static void TestMethod10()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

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

            // Prepare for Parameter Rebinding
            // We need to tell our rebinder to replace 'iParam' and 'jParam'
            // with the new 'xParam' in their respective bodies.
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

            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();
            
            List<Expression<Func<int , bool>>> delegateExpressions = new List<Expression<Func<int , bool>>>();

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `AND` operation (without short circuit) to the parameter value.
                Expression.And(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `AND` operation (without short circuit) to the parameter value.
                Expression.AndAssign(
                    parameterExpression ,
                    Expression.Constant(2)
                )
           );


            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `OR` operation (without short circuit) to the parameter value.
                Expression.Or(
                    parameterExpression ,
                    Expression.Constant(1)
                )
           );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `OR` operation (without short circuit) to the parameter value.
                Expression.OrAssign(
                    parameterExpression ,
                    Expression.Constant(4)
                )
           );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `XOR` operation (without short circuit) to the parameter value.
                Expression.ExclusiveOr(
                    parameterExpression ,
                    Expression.Constant(4)
                )
            );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise `XOR` operation (without short circuit) to the parameter value.
                Expression.ExclusiveOrAssign(
                    parameterExpression ,
                    Expression.Constant(4)
                )
            );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise left-shift operation (without short circuit) to the parameter value.
                Expression.LeftShift(
                    parameterExpression ,
                    Expression.Constant(4)
                )
            );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise left-shift operation (without short circuit) to the parameter value.
                Expression.LeftShiftAssign(
                    parameterExpression ,
                    Expression.Constant(4)
                )
            );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise right-shift operation (without short circuit) to the parameter value.
                Expression.RightShift(
                    parameterExpression ,
                    Expression.Constant(4)
                )
            );

            binaryExpressions.Add(
                // This expression represents a lambda expression
                // that do bitwise right-shift operation (without short circuit) to the parameter value.
                Expression.RightShiftAssign(
                    parameterExpression ,
                    Expression.Constant(4)
                )
            );

            delegateExpressions.Add(
                // This expression represents a lambda expression
                // that do logical `AND` operation (with short circuit) to the parameter value.
                Expression.Lambda<Func<int , bool>>(
                     Expression.AndAlso(
                        body1Rebound ,
                        body2Rebound
                     ) ,
                     xParam // The new lambda's single parameter
                )
           );

            delegateExpressions.Add(
                // This expression represents a lambda expression
                // that do logical `OR` operation (with short circuit) to the parameter value.
                Expression.Lambda<Func<int , bool>>(
                     Expression.OrElse(
                        body1Rebound ,
                        body2Rebound
                     ) ,
                     xParam // The new lambda's single parameter
                )
           );

            Console.WriteLine("About `binaryExpressions`");
            binaryExpressions.ForEach(
                (binaryExpression) =>
                {
                    string infoText = binaryExpression.GetInfo();
                    Console.WriteLine(infoText);
                    LambdaExpression lambdaExpression = Expression.Lambda(
                                                            binaryExpression ,
                                                            new List<ParameterExpression>() { parameterExpression }
                                                        );
                    var compiledDelegateExpression = lambdaExpression.Compile();
                    Console.WriteLine("---- new round ----");
                    for(int i = -5; i <= 5; i++) 
                    { 
                        var result = compiledDelegateExpression.DynamicInvoke(i);                  
                        Console.WriteLine("compiledDelegateExpression.DynamicInvoke({0}):{1}",i,result);
                    }
                }
            );
            Console.WriteLine();

            Console.WriteLine("About `delegateExpressions`");
            delegateExpressions.ForEach(
               (delegateExpression) =>
               {
                   var compiledDelegateExpression =  delegateExpression.Compile();
                   Console.WriteLine("---- new round ----");
                   for(int i=-5;i<=5;i++) 
                   {
                       var result = compiledDelegateExpression.DynamicInvoke(i);
                       Console.WriteLine("compiledDelegateExpression.DynamicInvoke({0}):{1}" , i , result);
                   }
               }
           );
            Console.WriteLine();
        }
```

where

`BinaryExpression.GetInfo` extension method is defined in `BinaryExpressionsExtensionMethods` static class.

`BinaryExpressionsExtensionMethods` static class in `ExpressionsExtensionMethods.cs`.

```
    public static class BinaryExpressionsExtensionMethods
    {
        public static string GetInfo(
            this BinaryExpression binaryExpression
        )
        {
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendFormat("binaryExpression.Left:{0}\n" , binaryExpression.Left);
            stringBuilder.AppendFormat("binaryExpression.Right:{0}\n" , binaryExpression.Right);
            stringBuilder.AppendFormat("binaryExpression.IsLifted:{0}\n" , binaryExpression.IsLifted);
            stringBuilder.AppendFormat("binaryExpression.IsLiftedToNull:{0}\n" , binaryExpression.IsLiftedToNull);
            return stringBuilder.ToString();
        }
    }
```

and 

the `ParameterRebinder` class inherits `ExpressionVisitor` abstract class

Defined as follows. 

`ParameterRebinder.cs`

```
    public class ParameterRebinder : ExpressionVisitor
    {
        private readonly Dictionary<ParameterExpression , ParameterExpression> _map;

        public ParameterRebinder(Dictionary<ParameterExpression , ParameterExpression> map)
        {
            _map = map ?? new Dictionary<ParameterExpression , ParameterExpression>();
        }

        // This method is called for each ParameterExpression encountered during the visit.
        protected override Expression VisitParameter(ParameterExpression p)
        {
            // If the parameter is in our map, return its replacement.
            if(_map.TryGetValue(p , out ParameterExpression replacement))
            {
                return replacement;
            }
            // Otherwise, return the original parameter.
            return base.VisitParameter(p);
        }
    }
```

It will output following

```
-----------------------------------------
In TestMethod10 method call,
About `binaryExpressions`
binaryExpression.Left:arg
binaryExpression.Right:1
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):1
compiledDelegateExpression.DynamicInvoke(-4):0
compiledDelegateExpression.DynamicInvoke(-3):1
compiledDelegateExpression.DynamicInvoke(-2):0
compiledDelegateExpression.DynamicInvoke(-1):1
compiledDelegateExpression.DynamicInvoke(0):0
compiledDelegateExpression.DynamicInvoke(1):1
compiledDelegateExpression.DynamicInvoke(2):0
compiledDelegateExpression.DynamicInvoke(3):1
compiledDelegateExpression.DynamicInvoke(4):0
compiledDelegateExpression.DynamicInvoke(5):1
binaryExpression.Left:arg
binaryExpression.Right:2
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):2
compiledDelegateExpression.DynamicInvoke(-4):0
compiledDelegateExpression.DynamicInvoke(-3):0
compiledDelegateExpression.DynamicInvoke(-2):2
compiledDelegateExpression.DynamicInvoke(-1):2
compiledDelegateExpression.DynamicInvoke(0):0
compiledDelegateExpression.DynamicInvoke(1):0
compiledDelegateExpression.DynamicInvoke(2):2
compiledDelegateExpression.DynamicInvoke(3):2
compiledDelegateExpression.DynamicInvoke(4):0
compiledDelegateExpression.DynamicInvoke(5):0
binaryExpression.Left:arg
binaryExpression.Right:1
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-5
compiledDelegateExpression.DynamicInvoke(-4):-3
compiledDelegateExpression.DynamicInvoke(-3):-3
compiledDelegateExpression.DynamicInvoke(-2):-1
compiledDelegateExpression.DynamicInvoke(-1):-1
compiledDelegateExpression.DynamicInvoke(0):1
compiledDelegateExpression.DynamicInvoke(1):1
compiledDelegateExpression.DynamicInvoke(2):3
compiledDelegateExpression.DynamicInvoke(3):3
compiledDelegateExpression.DynamicInvoke(4):5
compiledDelegateExpression.DynamicInvoke(5):5
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-1
compiledDelegateExpression.DynamicInvoke(-4):-4
compiledDelegateExpression.DynamicInvoke(-3):-3
compiledDelegateExpression.DynamicInvoke(-2):-2
compiledDelegateExpression.DynamicInvoke(-1):-1
compiledDelegateExpression.DynamicInvoke(0):4
compiledDelegateExpression.DynamicInvoke(1):5
compiledDelegateExpression.DynamicInvoke(2):6
compiledDelegateExpression.DynamicInvoke(3):7
compiledDelegateExpression.DynamicInvoke(4):4
compiledDelegateExpression.DynamicInvoke(5):5
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-1
compiledDelegateExpression.DynamicInvoke(-4):-8
compiledDelegateExpression.DynamicInvoke(-3):-7
compiledDelegateExpression.DynamicInvoke(-2):-6
compiledDelegateExpression.DynamicInvoke(-1):-5
compiledDelegateExpression.DynamicInvoke(0):4
compiledDelegateExpression.DynamicInvoke(1):5
compiledDelegateExpression.DynamicInvoke(2):6
compiledDelegateExpression.DynamicInvoke(3):7
compiledDelegateExpression.DynamicInvoke(4):0
compiledDelegateExpression.DynamicInvoke(5):1
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-1
compiledDelegateExpression.DynamicInvoke(-4):-8
compiledDelegateExpression.DynamicInvoke(-3):-7
compiledDelegateExpression.DynamicInvoke(-2):-6
compiledDelegateExpression.DynamicInvoke(-1):-5
compiledDelegateExpression.DynamicInvoke(0):4
compiledDelegateExpression.DynamicInvoke(1):5
compiledDelegateExpression.DynamicInvoke(2):6
compiledDelegateExpression.DynamicInvoke(3):7
compiledDelegateExpression.DynamicInvoke(4):0
compiledDelegateExpression.DynamicInvoke(5):1
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-80
compiledDelegateExpression.DynamicInvoke(-4):-64
compiledDelegateExpression.DynamicInvoke(-3):-48
compiledDelegateExpression.DynamicInvoke(-2):-32
compiledDelegateExpression.DynamicInvoke(-1):-16
compiledDelegateExpression.DynamicInvoke(0):0
compiledDelegateExpression.DynamicInvoke(1):16
compiledDelegateExpression.DynamicInvoke(2):32
compiledDelegateExpression.DynamicInvoke(3):48
compiledDelegateExpression.DynamicInvoke(4):64
compiledDelegateExpression.DynamicInvoke(5):80
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-80
compiledDelegateExpression.DynamicInvoke(-4):-64
compiledDelegateExpression.DynamicInvoke(-3):-48
compiledDelegateExpression.DynamicInvoke(-2):-32
compiledDelegateExpression.DynamicInvoke(-1):-16
compiledDelegateExpression.DynamicInvoke(0):0
compiledDelegateExpression.DynamicInvoke(1):16
compiledDelegateExpression.DynamicInvoke(2):32
compiledDelegateExpression.DynamicInvoke(3):48
compiledDelegateExpression.DynamicInvoke(4):64
compiledDelegateExpression.DynamicInvoke(5):80
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-1
compiledDelegateExpression.DynamicInvoke(-4):-1
compiledDelegateExpression.DynamicInvoke(-3):-1
compiledDelegateExpression.DynamicInvoke(-2):-1
compiledDelegateExpression.DynamicInvoke(-1):-1
compiledDelegateExpression.DynamicInvoke(0):0
compiledDelegateExpression.DynamicInvoke(1):0
compiledDelegateExpression.DynamicInvoke(2):0
compiledDelegateExpression.DynamicInvoke(3):0
compiledDelegateExpression.DynamicInvoke(4):0
compiledDelegateExpression.DynamicInvoke(5):0
binaryExpression.Left:arg
binaryExpression.Right:4
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False

---- new round ----
compiledDelegateExpression.DynamicInvoke(-5):-1
compiledDelegateExpression.DynamicInvoke(-4):-1
compiledDelegateExpression.DynamicInvoke(-3):-1
compiledDelegateExpression.DynamicInvoke(-2):-1
compiledDelegateExpression.DynamicInvoke(-1):-1
compiledDelegateExpression.DynamicInvoke(0):0
compiledDelegateExpression.DynamicInvoke(1):0
compiledDelegateExpression.DynamicInvoke(2):0
compiledDelegateExpression.DynamicInvoke(3):0
compiledDelegateExpression.DynamicInvoke(4):0
compiledDelegateExpression.DynamicInvoke(5):0

About `delegateExpressions`
---- new round ----
i:-5
compiledDelegateExpression.DynamicInvoke(-5):False
i:-4
compiledDelegateExpression.DynamicInvoke(-4):False
i:-3
compiledDelegateExpression.DynamicInvoke(-3):False
i:-2
compiledDelegateExpression.DynamicInvoke(-2):False
i:-1
compiledDelegateExpression.DynamicInvoke(-1):False
i:0
compiledDelegateExpression.DynamicInvoke(0):False
i:1
j:1
compiledDelegateExpression.DynamicInvoke(1):False
i:2
j:2
compiledDelegateExpression.DynamicInvoke(2):False
i:3
j:3
compiledDelegateExpression.DynamicInvoke(3):False
i:4
j:4
compiledDelegateExpression.DynamicInvoke(4):False
i:5
j:5
compiledDelegateExpression.DynamicInvoke(5):False
---- new round ----
i:-5
j:-5
compiledDelegateExpression.DynamicInvoke(-5):True
i:-4
j:-4
compiledDelegateExpression.DynamicInvoke(-4):True
i:-3
j:-3
compiledDelegateExpression.DynamicInvoke(-3):True
i:-2
j:-2
compiledDelegateExpression.DynamicInvoke(-2):True
i:-1
j:-1
compiledDelegateExpression.DynamicInvoke(-1):True
i:0
j:0
compiledDelegateExpression.DynamicInvoke(0):True
i:1
compiledDelegateExpression.DynamicInvoke(1):True
i:2
compiledDelegateExpression.DynamicInvoke(2):True
i:3
compiledDelegateExpression.DynamicInvoke(3):True
i:4
compiledDelegateExpression.DynamicInvoke(4):True
i:5
compiledDelegateExpression.DynamicInvoke(5):True

```

## reference
### API docs
+ [`Expression.AndAlso Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.andalso?view=net-8.0)
+ [`Expression.OrElse Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.orelse?view=net-8.0)
