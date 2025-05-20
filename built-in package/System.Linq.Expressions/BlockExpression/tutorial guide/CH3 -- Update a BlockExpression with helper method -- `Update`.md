# CH3 -- Update a BlockExpression with helper method -- `Update`
## objectives
You will learn how to

+ update a `BlockExpression` with helper method -- `Update`

## CH3.1 -- update a `BlockExpression` with helper method -- `Update`
You can update a `BlockExpression` with helper method -- `Update` instance method call.

Let's see the declaration first. 

```
public System.Linq.Expressions.BlockExpression Update(
  System.Collections.Generic.IEnumerable<System.Linq.Expressions.ParameterExpression>? variables,
  System.Collections.Generic.IEnumerable<System.Linq.Expressions.Expression> expressions
);
```

You can know that 

+ 0th argument can be null.
+ 0th argument indicates a list of variables for updating.
+ 1th argument can NOT be null.
+ 1th argument indicates a list of expression for updating.

> [!CAUTION]
> Watch out for consistency for type of **last expression**.
>
> See following restrictions.

By [Google Gemini's answer](https://g.co/gemini/share/388dc7303027), it discussed the restriction of `Update` instance call in summary.

<img width="473" alt="image" src="https://github.com/user-attachments/assets/7445fc26-cf3d-4ff7-b779-c165df5f0997" />

If you want to update the `BlockExpression` instance using `Update` instance call, it MUST follow these requirements. 

Otherwise, it will throw an exception at runtime (such as `System.ArgumentException`). 

+ consistent type: the type of **last expression** before updating MUST be consistent with that after updating.

see wrong example 1.

## examples
### example 1
Invoking following method 

```
        public static void TestMethod7()
        {
            ParameterExpression variable1 = Expression.Variable(typeof(int) , "x");
            ParameterExpression variable2 = Expression.Variable(typeof(string) , "message");
            BlockExpression blockExpr = Expression.Block(
                new [ ] { variable1 , variable2 } ,
                Expression.Assign(variable1 , Expression.Constant(10)) ,
                Expression.Assign(variable2 , Expression.Constant("Hello"))
            );

            Console.WriteLine("Before updating the BlockExpression using `Update` instance method call");
            blockExpr.PrintInfo();

            ParameterExpression variable3 = Expression.Variable(typeof(string) , "greetingMessage");

            BinaryExpression expression1 = Expression.Assign(variable3 , Expression.Constant("Hello"));
            
            List<ParameterExpression> newVariables = new List<ParameterExpression>();
            newVariables.AddRange(blockExpr.Variables);
            newVariables.Add(variable3);

            List<Expression> newExpressions = new List<Expression>();
            newExpressions.AddRange(blockExpr.Expressions);
            newExpressions.Add(expression1);

            BlockExpression newBlockExpr = blockExpr.Update(
                newVariables,
                newExpressions
            );

            Console.WriteLine("After updating the BlockExpression using `Update` instance method call");
            newBlockExpr.PrintInfo();
        }
```

where 

`PrintInfo` extension method is defined as follows.

```
        public static void PrintInfo(
            this BlockExpression blockExpr
        )
        {
            Console.WriteLine($"Result of last expression:{blockExpr.Result}");
            Console.WriteLine($"Variable of theses expressions:");
            var variables = blockExpr.Variables;
            foreach(var variable in variables)
            {
                Console.WriteLine(variable.ToString());
            }
            Console.WriteLine($"Theses expressions:");
            var expressions = blockExpr.Expressions;
            foreach(var expr in expressions)
            {
                Console.WriteLine(expr.ToString());
            }
            Console.WriteLine($"static type of the expression:{blockExpr.Type}");
        }
```

It will output following

```
Before updating the BlockExpression using `Update` instance method call
Result of last expression:(message = "Hello")
Variable of theses expressions:
x
message
Theses expressions:
(x = 10)
(message = "Hello")
static type of the expression:System.String
After updating the BlockExpression using `Update` instance method call
Result of last expression:(greetingMessage = "Hello")
Variable of theses expressions:
x
message
greetingMessage
Theses expressions:
(x = 10)
(message = "Hello")
(greetingMessage = "Hello")
static type of the expression:System.String
```

#### demo project

## wrong examples
### wrong example 1
Invoking following will cause `System.ArgumentException`, since it violate the restriction -- consistent type of **last expression**.

```
        public static void TestMethod8()
        {
            ParameterExpression variable1 = Expression.Variable(typeof(int) , "x");
            ParameterExpression variable2 = Expression.Variable(typeof(string) , "message");
            BlockExpression blockExpr = Expression.Block(
                new [ ] { variable1 , variable2 } ,
                Expression.Assign(variable1 , Expression.Constant(10)) ,
                Expression.Assign(variable2 , Expression.Constant("Hello"))
            );

            Console.WriteLine("Before updating the BlockExpression using `Update` instance method call");
            blockExpr.PrintInfo();

            Expression expression1 = Expression.Call(
                    null ,
                    typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(String) }) ,
                    Expression.Constant("World!")
                );
           
            List<Expression> newExpressions = new List<Expression>();
            newExpressions.AddRange(blockExpr.Expressions);
            newExpressions.Add(expression1);

            BlockExpression newBlockExpr = blockExpr.Update(
                blockExpr.Variables ,
                newExpressions
            );

            Console.WriteLine("After updating the BlockExpression using `Update` instance method call");
            newBlockExpr.PrintInfo();
        }
```

![Uploading image.png…]()

#### demo project
