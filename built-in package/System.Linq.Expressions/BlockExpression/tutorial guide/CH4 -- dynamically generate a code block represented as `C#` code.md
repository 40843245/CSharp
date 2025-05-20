# CH4 -- dynamically generate a code block represented as `C#` code
## objectives
You will know how to 

+ dynamically generate a code block represented as `C#` code

## CH4.1 -- dynamically generate a code block represented as `C#` code
Step 1:

Create an expression tree.

```
BlockExpression blockExpr = Expression.Block(
                // ... expressions
            );
```

Step 2:

Gets the expressions.

```
var expressions = blockExpr.Expressions;
```

Step 3:

Then iterate the element one-by-one, for each element, converting it to string using `ToString` instance method call.

## examples
### example 1
Excute this method

```
        public static void TestMethod1()
        {
            BlockExpression blockExpr = Expression.Block(
                Expression.Call(
                    null ,
                    typeof(Console).GetMethod("Write" , new Type [ ] { typeof(String) }) ,
                    Expression.Constant("Hello ")
                   ) ,
                Expression.Call(
                    null ,
                    typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(String) }) ,
                    Expression.Constant("World!")
                    ) ,
                Expression.Constant(42)
            );
            StringBuilder stringBuilder = new StringBuilder();
            Console.WriteLine("blockExpr represents C# code");
            var expressions = blockExpr.Expressions;
            foreach(var expr in expressions)
            {
                stringBuilder.AppendLine(expr.ToString());
            }
            string codeBlockText = stringBuilder.ToString();
            Console.Write(codeBlockText);
        }
```

will output the following:

```
blockExpr represents C# code:
Write("Hello ")
WriteLine("World!")
42
```

### demo project 
See [example 1]()
