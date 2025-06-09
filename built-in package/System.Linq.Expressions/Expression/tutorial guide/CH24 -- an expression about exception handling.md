# CH24 -- an expression about exception handling
## objectives
You will learn how to

+ create an expression about exception handling in other way

## CH24.1 -- create an expression about exception handling in other way
> [!NOTE]
> For the first approach (such as invoking `Expression.TryCatch` static method)
>
> See CH17.

### `Expression.MakeTry` static method

syntax:

```
public static System.Linq.Expressions.TryExpression MakeTry(
    Type? type, 
    System.Linq.Expressions.Expression body, 
    System.Linq.Expressions.Expression? finally, 
    System.Linq.Expressions.Expression? fault, 
    System.Collections.Generic.IEnumerable<System.Linq.Expressions.CatchBlock>? handlers
);
```

## examples
### example 1
Invoking following method

```
       /// <summary>
        /// illustrates how to use `Expression.MakeTry` with a return value.
        /// </summary>
        public static void TestMethod40()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // 1. Define parameter for the divisor for the lambda function
            ParameterExpression divisorParameter = Expression.Parameter(typeof(int) , "divisor");

            // 2. Define a variable to store the result of the try/catch operations.
            // This variable will hold the final return value of the lambda.
            ParameterExpression resultVariable = Expression.Variable(typeof(int) , "result");

            // 3. Define a label for the overall return of the lambda.
            // This label will be the target for the final return value.
            LabelTarget returnLabel = Expression.Label(typeof(int) , "returnLabel");

            // --- Body of the 'try' block ---
            // The try block will assign the result of the division to resultVariable.
            // It should NOT contain an Expression.Return directly.
            Expression assignResult = Expression.Assign(resultVariable , Expression.Divide(Expression.Constant(100) , divisorParameter));

            Expression tryBody = Expression.Block(
                assignResult // Assign the successful division result to resultVariable
            );

            // --- Body of the 'catch' block (for DivideByZeroException) ---
            // The catch block will handle the exception and assign an error value to resultVariable.
            // It should NOT contain an Expression.Return directly.
            ParameterExpression exParameter = Expression.Parameter(typeof(DivideByZeroException) , "ex");
            Expression catchBody = Expression.Block(
                Expression.Call(
                    null , // static method
                    typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) }) ,
                    Expression.Constant("Caught DivideByZeroException!")
                ) ,
                Expression.Assign(resultVariable , Expression.Constant(0)) // Assign 0 to resultVariable on error
            );

            // Create the CatchBlock for DivideByZeroException
            CatchBlock divideByZeroCatchBlock = Expression.Catch(
                exParameter ,
                catchBody
            );

            // --- Body of the 'finally' block ---
            // The finally block performs cleanup and should NOT contain an Expression.Return.
            // It does not affect the return value of the overall expression.
            Expression finallyBody =
                Expression.Block(
                    Expression.Call(
                        null ,
                        typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) }) ,
                        Expression.Constant("Finally block executed.")
                    )
                );

            // --- Create the TryExpression using MakeTry ---
            // The return type of the TryExpression should match the type of resultVariable.
            TryExpression tryExpression = Expression.MakeTry(
                typeof(int) , // The return type of the try-catch-finally block itself.
                              // This means the block will produce an int value (implicitly resultVariable).
                tryBody ,
                finallyBody , // The finally block
                null ,        // No fault block in this example
                new [ ] { divideByZeroCatchBlock } // The catch blocks
            );

            // Add the label target to the overall block.
            // The overallBlock is the body of the lambda.
            // The last expression in overallBlock determines the lambda's return value.
            BlockExpression overallBlock = Expression.Block(
                new [ ] { resultVariable } , // Variables declared in this block (resultVariable)
                tryExpression ,            // The try-catch-finally structure
                Expression.Label(returnLabel , resultVariable) // The final return point, using the value of resultVariable
            );

            // Compile and execute the expression tree
            // The lambda's return type is int, which matches the type of resultVariable
            // and the LabelTarget.
            Func<int , int> divideFunction = Expression.Lambda<Func<int , int>>(
                overallBlock ,
                divisorParameter
            ).Compile();

            Console.WriteLine("--- Test 1: Successful Division (divisor = 5) ---");
            int result1 = divideFunction(5);
            Console.WriteLine($"Result: {result1}");

            Console.WriteLine("\n--- Test 2: Division by Zero (divisor = 0) ---");
            int result2 = divideFunction(0);
            Console.WriteLine($"Result: {result2}");
        }
```

will output following

```
In TestMethod40 method call,
--- Test 1: Successful Division (divisor = 5) ---
Finally block executed.
Result: 20

--- Test 2: Division by Zero (divisor = 0) ---
Caught DivideByZeroException!
Finally block executed.
Result: 0
```

### example 2
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.CatchBlock`.
        /// </summary>
        public static void TestMethod41()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // 1. Define a parameter for the exception.
            // This is equivalent to 'Exception ex' in a C# catch block: catch (Exception ex) { ... }
            ParameterExpression exceptionParameter = Expression.Parameter(typeof(Exception) , "ex");

            // 2. Define the body of the catch block.
            // This is the code that will execute when the exception is caught.
            // For this example, we'll just print the exception message to the console.
            MethodInfo writeLineMethod = typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) });

            // Access the Message property of the caught exception: ex.Message
            MemberExpression exceptionMessage = Expression.Property(exceptionParameter , "Message");

            // Call Console.WriteLine(ex.Message)
            Expression catchBody =
                Expression.Block(
                    typeof(int) ,
                    new List<Expression>()
                    {
                        Expression.Call(
                            typeof(string).GetMethod("Concat" , new [ ] { typeof(object) , typeof(object) }) , // Or other overloads as needed
                            Expression.Constant("catch DivideByZeroException error!") ,
                            Expression.Convert(exceptionMessage , typeof(object))
                        ),
                        Expression.Constant(0)
                    }
                );

            CatchBlock generalCatchBlock = Expression.Catch(
                exceptionParameter, // The type of exception to catch and the variable to hold it
                catchBody           // The expression to execute when this exception is caught
            );

            Console.WriteLine("Successfully created a CatchBlock using Expression.Catch.");
            Console.WriteLine($"  Exception Type: {generalCatchBlock.Test.Name}");
            Console.WriteLine($"  Exception Parameter: {generalCatchBlock.Variable.Name}");
            Console.WriteLine($"  Catch Body Type: {generalCatchBlock.Body.Type.Name}");

            // --- Example of how this CatchBlock would be used in a TryExpression ---

            // Define a variable for the result of the try block
            ParameterExpression resultVar = Expression.Variable(typeof(int) , "result");

            // Try body: A simple division that might throw an exception
            Expression tryBody = Expression.Block(
                Expression.Assign(resultVar , Expression.Divide(Expression.Constant(10) , Expression.Constant(0))) // Division by zero
            );

            // Finally body (optional for this example, but good practice)
            Expression finallyBody = Expression.Call(
                null ,
                writeLineMethod ,
                Expression.Constant("Finally block executed.")
            );

            // Create the TryExpression using Expression.MakeTry
            TryExpression tryExpression = Expression.MakeTry(
                typeof(int) , // Return type of the try expression
                tryBody ,
                finallyBody ,
                null , // No fault block
                new [ ] { generalCatchBlock } // Include our created CatchBlock
            );

            // Overall block for the lambda
            BlockExpression overallBlock = Expression.Block(
                new [ ] { resultVar } , // Declare resultVar within this block's scope
                tryExpression ,
                resultVar // The last expression in the block determines its return value
            );

            // Compile and execute the expression tree
            Console.WriteLine("\nCompiling and executing a lambda with the CatchBlock...");
            Func<int> compiledLambda = Expression.Lambda<Func<int>>(overallBlock).Compile();

            try
            {
                int result = compiledLambda();
                Console.WriteLine($"Lambda execution result: {result}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"An unexpected exception occurred during lambda execution: {ex.Message}");
            }
        }
```

will output following

```
In TestMethod41 method call,
Successfully created a CatchBlock using Expression.Catch.
  Exception Type: Exception
  Exception Parameter: ex
  Catch Body Type: Int32

Compiling and executing a lambda with the CatchBlock...
Finally block executed.
Lambda execution result: 0In TestMethod41 method call,
Successfully created a CatchBlock using Expression.Catch.
  Exception Type: Exception
  Exception Parameter: ex
  Catch Body Type: Int32

Compiling and executing a lambda with the CatchBlock...
Finally block executed.
Lambda execution result: 0
```

## reference
### API docs
+ [`Expression.MakeTry Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.maketry?view=net-8.0)
+ [`Expression.Return Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.return?view=net-8.0)
+ [`TryExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.tryexpression?view=net-8.0)

### examples
+ code in example 1 are generated by it and then fixed by [Google Gemini Flash 2.5](https://g.co/gemini/share/42e61b0790d3)

+ code in example 2 are generated by it and then fixed by [Google Gemini Flash 2.5](https://g.co/gemini/share/86c5e4c02712)