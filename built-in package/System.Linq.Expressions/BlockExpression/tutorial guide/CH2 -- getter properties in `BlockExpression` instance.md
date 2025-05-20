# CH2 -- getter properties in `BlockExpression` instance
## objectives
You will know

+ some getter properties in in `BlockExpression` instance

## CH2.1 -- some getter properties in in `BlockExpression` instance
<img width="439" alt="image" src="https://github.com/user-attachments/assets/cdbc2e76-26ee-4cce-a7d3-bb798e884d8e" />

### `Expressions`
Gets the expressions in this block.

see example 1.

### `Result`
Gets the last expression in this block.

see example 1.

### `Type`
Gets the static type of the expression.

Although the docs says `Type` getter properties gets the static type of the expression,

I found its definition from [BlockExpression.cs file (source code)](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Linq.Expressions/src/System/Linq/Expressions/BlockExpression.cs#L57)

And it seems to get type of the last expression.

<img width="956" alt="image" src="https://github.com/user-attachments/assets/9374da02-0fa1-44bf-a410-f2eafa221272" />

see example 1.

### `Variables`
Gets the variables defined in this block.

see example 1 and example 2 then compare them.

## examples
### example 1
Invoking following method

```
        public static void TestMethod5()
        {
            string welcomeMessage = "Hello ";
            BlockExpression blockExpr = Expression.Block(
                Expression.Call(
                    null ,
                    typeof(Console).GetMethod("Write" , new Type [ ] { typeof(String) }) ,
                    Expression.Constant(welcomeMessage)
                   ),
                Expression.Call(
                    null ,
                    typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(String) }) ,
                    Expression.Constant("World!")
                    ),
                Expression.Constant(42)
            );

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

will output following

```
Result of last expression:42
Variable of theses expressions:
Theses expressions:
Write("Hello ")
WriteLine("World!")
42
static type of the expression:System.Int32
```

Analysis:

+ last expression: `Expression.Constant(42)` which is evaluated to `42` with data type `System.Int32`, so `blockExpr.Result` returns `42`.
+ There are no defined variables inside `Expression.block` factory method call, even `welcomeMessage` variable is placed (`Expression.Constant(welcomeMessage)`) in the first expression, so `blockExpr.Variable` returns empty `ICollection`.
+ Inside `Expression.block` factory method call, there is only one static type in these expressions -- in the third expression `Expression.Constant(42)` which is evaluated to `42` with data type `System.Int32`, so `blockExpr.Type` is `System.Int32`.

#### demo project
see [Example.7z (version 1.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/built-in%20package/System.Linq.Expressions/BlockExpression/code/v1.0.0/Example.7z)

### example 2
Invoking following method

```
        public static void TestMethod6()
        {
            ParameterExpression variable1 = Expression.Variable(typeof(int) , "x");
            ParameterExpression variable2 = Expression.Variable(typeof(string) , "message");
            BlockExpression blockExpr = Expression.Block(
                new [ ] { variable1 , variable2 } ,
                Expression.Assign(variable1 , Expression.Constant(10)) ,
                Expression.Assign(variable2 , Expression.Constant("Hello"))
            );

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

will output following

```
Result of last expression:(message = "Hello")
Variable of theses expressions:
x
message
Theses expressions:
(x = 10)
(message = "Hello")
static type of the expression:System.String
```

Analysis:

+ In `ParameterExpression variable1 = Expression.Variable(typeof(int) , "x");`, we can easily found that it invoke the overloaded method [`Variable(Type, String)`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.variable?view=net-8.0#system-linq-expressions-expression-variable(system-type-system-string)).

In the overloaded method call, 

it passes data type `System.Int32` as 0th argument (zero-based), meaning the variable's data type is `System.Int32`.

While it passes `"x"` in 1th argument, meaning that the variable name is `x`.

+ Similarly, in `ParameterExpression variable2 = Expression.Variable(typeof(string) , "message");`

it creates an expression and defines a variable with name `message` and data type `string`

+ In `new [ ] { variable1 , variable2 }` inside `BlockExpression blockExpr = Expression.Block(...);`, it contains two expressions -- `variable1` , `variable2`

+ In `Expression.Assign(variable1 , Expression.Constant(10))` inside `BlockExpression blockExpr = Expression.Block(...);`, it assigns the defined variable in expression `variable1` as `10`.

+ Similarly, in `Expression.Assign(variable2 , Expression.Constant("Hello"))` inside `BlockExpression blockExpr = Expression.Block(...);`, it assigns the defined variable in expression `variable1` as `"Hello"`.

+ So `blockExpr.Variable` will return `x` and `message`

+ And `blockExpr.Expressions` will return `(x = 10)` and `(message = "Hello")`

+ By definition of `Type` getter-property, it will return the type of last expression which is `System.String`.
  
#### demo project
see [Example.7z (version 1.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/built-in%20package/System.Linq.Expressions/BlockExpression/code/v1.0.0/Example.7z)

## reference
### API docs
[`BlockExpression` API](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.blockexpression?view=net-9.0)
