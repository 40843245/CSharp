# CH3 -- compile the dynamically generated code block and execute it
## objectives
You will learn how to 

+ compile dynamically generated code block
+ get the result after executing the compiled result from dynamically generated code block

## CH3.1 -- compile dynamically generated code block
Step 1:

Creates an expression tree that represents a lambda expression.

```
BlockExpression blockExpr = Expression.Block(
  // ... expressions
);

// given `blockExpr` expressions, create an expression tree that matches the first expression whose
// generic delegate type represents a method that takes no input parameters and returns an `int` (implied by `Func<int>`).
Expression.Lambda<Func<int>>(blockExpr); 
```

Step 2:

Then invoke `Compile` instance method as follows.

```
Expression.Lambda<Func<int>>(blockExpr).Compile(); 
```

Then it will return a delegate function.

## CH3.2 -- get the result after executing the compiled result from dynamically generated code block
Step 1 and 2 are same as previous section.

Step 3:

Invoke the delegate function as following

```
Expression.Lambda<Func<int>>(blockExpr).Compile()();
```

or 

```
Expression.Lambda<Func<int>>(blockExpr).Compile().Invoke();
```

It will evaluate the delegate function and return the result.

## examples
### example 1
Invoking the following method

```
        public static void TestMethod3()
        {
            // Add the following directive to your file:
            // using System.Linq.Expressions;

            // The block expression allows for executing several expressions sequentually.
            // When the block expression is executed,
            // it returns the value of the last expression in the sequence.
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

            // The following statement first creates an expression tree,
            // then compiles it, returning a delegate function.
            var compiledBlockExpr = Expression.Lambda<Func<int>>(blockExpr).Compile();
            Console.WriteLine("After compiling the block expression:");
            Console.WriteLine(compiledBlockExpr);

            // Then execute it
            var result = compiledBlockExpr();
            
            Console.WriteLine("The return value of the block expression:");
            Console.WriteLine(result);
        }
```

will output following

```
After compiling the block expression:
System.Func`1[System.Int32]
Hello World!
The return value of the block expression:
42
```
