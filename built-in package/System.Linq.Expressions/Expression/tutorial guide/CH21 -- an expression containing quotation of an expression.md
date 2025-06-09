# CH21 -- an expression containing quotation of an expression
## objectives
You will learn how to

+ create an expression containing quotation of an expression

## CH21.1 -- create an expression containing quotation of an expression
### `Expression.Quote` static method
creates a `UnaryExpression` that represents an expression that has a constant value of type `Expression`.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.Quote`.
        /// </summary>
        public static void TestMethod34()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // 1. Define an inner lambda expression:
            //    This lambda takes an int `x` and returns `x * 2`
            Expression<Func<int , int>> multiplyByTwo = x => x * 2;

            Console.WriteLine($"Inner Lambda (C# code): {multiplyByTwo}");

            // 2. Now, let's create an outer expression that *returns* this inner lambda.

            // Correct way using Expression.Quote:
            // We want an outer expression that produces the 'multiplyByTwo' Expression tree.
            // The type of the value returned by the outer expression is Expression<Func<int, int>>.
            Expression<Func<Expression<Func<int , int>>>> outerLambda =
                Expression.Lambda<Func<Expression<Func<int , int>>>>(
                    Expression.Quote(multiplyByTwo)
                );

            Console.WriteLine($"Outer Lambda (with Quote): {outerLambda}");

            // 3. Compile and execute the outer expression:
            Func<Expression<Func<int , int>>> compiledOuterLambda =
                outerLambda.Compile();

            // 4. The result of executing the outer lambda is the *expression tree* itself:
            Expression<Func<int , int>> retrievedInnerExpression = compiledOuterLambda();

            Console.WriteLine($"Retrieved Inner Expression (from compiled outer): {retrievedInnerExpression}");

            // 5. You can then compile and execute the retrieved inner expression:
            Func<int , int> compiledInnerFunction = retrievedInnerExpression.Compile();
            int result = compiledInnerFunction(5);
            Console.WriteLine($"Result of inner expression (5 * 2): {result}"); 
        }
```

will output following

```
In TestMethod34 method call,
Inner Lambda (C# code): x => (x * 2)
Outer Lambda (with Quote): () => x => (x * 2)
Retrieved Inner Expression (from compiled outer): x => (x * 2)
Result of inner expression (5 * 2): 10
```

## reference
### API docs
+ [`Expression.Quote(Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.quote?view=net-8.0)