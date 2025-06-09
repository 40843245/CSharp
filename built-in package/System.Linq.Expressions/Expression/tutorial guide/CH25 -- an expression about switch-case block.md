# CH25 -- an expression about `switch-case` block
## objectives
You will learn keywords that used in

+ an expression about `switch-case` block

## CH25.1 -- an expression about `switch-case` block
### `Expression.SwitchCase` static method
creates a `Expressions.SwitchCase` object to be used in a `Expressions.SwitchExpression` object, representing a `case` block.

### `Expression.Switch` static method
creates a `SwitchExpression` that represents a switch statement.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.Switch`.
        /// </summary>
        public static void TestMethod43()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // 1. Define a parameter for the switch statement (the value being switched on)
            ParameterExpression value = Expression.Parameter(typeof(string) , "value");

            // 2. Define labels for the 'break' equivalent within the switch cases
            //    (though for simple return types, the result variable handles it)
            LabelTarget returnLabel = Expression.Label(typeof(string));

            // 3. Create the default case for the switch
            Expression defaultBody = Expression.Constant("Unknown value" , typeof(string));

            // 4. Create the switch cases
            SwitchCase case1 = Expression.SwitchCase(
                Expression.Constant("Value is 1") , // Body of the case
                Expression.Constant("1", typeof(string))             // Test value for the case
            );

            SwitchCase case2 = Expression.SwitchCase(
                Expression.Constant("Value is 2") ,
                Expression.Constant("2", typeof(string))
            );

            SwitchCase case3 = Expression.SwitchCase(
                Expression.Constant("Value is 3") ,
                Expression.Constant("3", typeof(string))
            );

            // 5. Construct the Switch expression
            Expression switchExpression = Expression.Switch(
                value ,             // The value being switched on
                defaultBody ,       // The default case expression
                case1 ,             // The individual switch cases
                case2 ,
                case3
            );

            // 6. Combine the switch expression with the return label to form a lambda
            //    This lambda will take an integer and return a string.
            Expression<Func<string , string>> lambda = 
                Expression.Lambda<Func<string , string>>(
                    Expression.Block(
                        switchExpression ,
                        Expression.Label(returnLabel , Expression.Constant("Default Fallback")) // Fallback if no return is hit
                    ) ,
                    value
            );

            // 7. Compile the expression tree into an executable delegate
            Func<string , string> compiledSwitch = lambda.Compile();

            string [ ] datas = new string [ ]
            {
                "-1",
                "0",
                "1",
                "2",
                "3",
                "4",
                "5",
                "NAN",
                "Spy x family"
            };

            foreach (string data in datas)
            {
                Console.WriteLine($"compiledSwitch({data}):{compiledSwitch(data)}");
            }
        }
```

will output following

```
In TestMethod43 method call,
compiledSwitch(-1):Default Fallback
compiledSwitch(0):Default Fallback
compiledSwitch(1):Default Fallback
compiledSwitch(2):Default Fallback
compiledSwitch(3):Default Fallback
compiledSwitch(4):Default Fallback
compiledSwitch(5):Default Fallback
compiledSwitch(NAN):Default Fallback
compiledSwitch(Spy x family):Default Fallback
```

### example 2
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.Switch`.
        /// </summary>
        public static void TestMethod44()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // An expression that represents the switch value.
            ConstantExpression switchValue = Expression.Constant(3);

            MethodInfo writeLineMethod = typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) });
            // This expression represents a switch statement
            // that has a default case.
            SwitchExpression switchExpression =
                Expression.Switch(
                    switchValue ,
                    Expression.Call(
                                null ,
                                writeLineMethod ,
                                Expression.Constant("Default")
                     ) ,
                     new SwitchCase [ ]
                     {
                        Expression.SwitchCase(
                            Expression.Call(
                                null,
                                writeLineMethod,
                                Expression.Constant("First")
                            ),
                            Expression.Constant(1)
                        ),
                        Expression.SwitchCase(
                            Expression.Call(
                                null ,
                                writeLineMethod ,
                                Expression.Constant("Second")
                            ),
                            Expression.Constant(2)
                        )
                    }    
                );

            // The following statement first creates an expression tree,
            // then compiles it, and then runs it.
            Expression.Lambda<Action>(switchExpression).Compile()();
        }
```

will output following

```
In TestMethod44 method call,
Default
```

## reference
### API docs
+ [`Expression.SwitchCase Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.switchcase?view=net-8.0)
+ [`Expression.Switch Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.switch?view=net-8.0)
+ [`SwitchCase Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.switchcase?view=net-8.0)
+ [`SwitchExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.switchexpression?view=net-8.0)

### examples
+ The code in example 1 are generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/a9abc6dd4aee) and then fixed by it.

+ The code in example 2 are referenced to an example in [`SwitchExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.switchexpression?view=net-8.0)