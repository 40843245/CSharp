# CH6 -- the order of visiting the expression tree
## objectives
You will know

+ the order of visiting the expression tree

## CH6.1 -- the order of visiting the expression tree
See following example.

## examples
### example 1
Invoking following method 

```
        public static void TestMethod4()
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

            Console.WriteLine("The result of executing the expression tree:");
            // The following statement first creates an expression tree,
            // then compiles it, and then executes it.
            var result = Expression.Lambda<Func<int>>(blockExpr).Compile()();

            // Print out the expressions from the block expression.
            Console.WriteLine("The expressions from the block expression:");
            foreach(var expr in blockExpr.Expressions)
            {
                Console.WriteLine(expr.ToString());
            }

            // Print out the result of the tree execution.
            Console.WriteLine("The return value of the block expression:");
            Console.WriteLine(result);
        }
```

will output following

```
The result of executing the expression tree:
Hello World!
The expressions from the block expression:
Write("Hello ")
WriteLine("World!")
42
The return value of the block expression:
42
```

About the analysis, see [Google Gemini's answer](https://g.co/gemini/share/853d32e555cc)

#### demo project
see [Example.7z (version 1.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/built-in%20package/System.Linq.Expressions/BlockExpression/code/v1.0.0)
