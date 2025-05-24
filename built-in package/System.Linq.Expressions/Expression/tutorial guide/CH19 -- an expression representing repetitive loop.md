# CH19 -- an expression representing repetitive loop
## objectives
You will learn how to

+ create an expression representing repetitive loop

## CH19.1 -- create an expression representing repetitive loop
Before discussed this, we have to talk about these static method.

+ `Expression.Continue` static method: creates a `GotoExpression` representing a continue statement.

+ `Expression.Break` static method: creates a GotoExpression representing a break statement.

+ `Expression.Label` static method: creates a LabelTarget representing a label which will be used in `GotoExpression` (represents actions such as `unconditional jump`, `continue`, `break`).

If you are NOT familiar with `Expression.Label`, please review CH18 first.

### `Expression.Loop` static method
To create an expression representing repetitive loop, 

we need to 

+ create one label representing `break` statement.
+ create an expression containing a variable, set its intial value, and updates its value after it finishes to run each loop and before checking conditions. 
+ create an expression containing a condition.
+ put the logic (using `Expression` class) into the loop.
+ create an expression representing repetitive loop

Let's get started to write code.

For example, I want to write the code

```
int i;
i=0;
do
{
    Console.WriteLine("i="+i);
    ++i;
}while(i<3);

```

we can think it like this:

define a infinitely loop but exits using `break` keyword when the condition (`i >= 3`) is true. 

```
int i;
i=0;
do
{
    Console.WriteLine("i="+i);
    ++i;
    if(i>=3){
        break;
    }
}while(true);
```

Step 1:

To create one label representing `break` statement,

we need to invoke `Expression.Label` static method as follows.

```
LabelTarget loopBreak = Expression.Label("loopBreak");
```

Step 2:

To create an expression containing a variable, 

we need to invoke `Expression.Variable` static method as follows.

```
ParameterExpression i = Expression.Variable(typeof(int) , "i");
```

Step 3:

To intializes its value to for example 0,

we need to create an expression using `Expression.Assign` static method as follows.

```
Expression initializeI = Expression.Assign(i , Expression.Constant(0));
```

Step 4:

To updates its value after it finishes to run each loop and before checking conditions.

```
Expression.PostIncrementAssign(i)
```

Step 5:

To create an expression containing a condition, for example, when the `i >= 3` condition is true, then exits the repetitive loop,

we need to invoke `Expression.GreaterThanOrEqual` inside `Expression.IfThen`

```
Expression.IfThen(
                    Expression.GreaterThanOrEqual(i , Expression.Constant(3)) ,
                    Expression.Break(loopBreak)
                )
```

Step 6:

To create an expression representing `Console.WriteLine("i="+1)`,

```
// Print "i=" + i
                Expression.Call(
                    typeof(Console).GetMethod("WriteLine" , new [ ] { typeof(string) }) ,
                    Expression.Call(
                        typeof(string).GetMethod("Concat" , new [ ] { typeof(object) , typeof(object) }) , // Or other overloads as needed
                        Expression.Constant("i=") ,
                        Expression.Convert(i , typeof(object)) // Convert 'i' to object for string.Concat(object, object)
                    )
                )
```

Step 7:

Combine them together.

```
        Expression loopBody = Expression.Block(
                // Print "i=" + i
                Expression.Call(
                    typeof(Console).GetMethod("WriteLine" , new [ ] { typeof(string) }) ,
                    Expression.Call(
                        typeof(string).GetMethod("Concat" , new [ ] { typeof(object) , typeof(object) }) , // Or other overloads as needed
                        Expression.Constant("i=") ,
                        Expression.Convert(i , typeof(object)) // Convert 'i' to object for string.Concat(object, object)
                    )
                ),
                // Postfix Increment to i
                Expression.PostIncrementAssign(i) ,
                // Check condition (i < 3) and break if false
                Expression.IfThen(
                    Expression.GreaterThanOrEqual(i , Expression.Constant(3)) ,
                    Expression.Break(loopBreak)
                )
            );
```

Step 8:

Create an expression containg a loop

```
            // 5. Create the LoopExpression
            LoopExpression loop = Expression.Loop(
                loopBody ,
                loopBreak // Associate the break label with the loop
            );
```

Step 9:

Combine initialization and the loop into a block expression

```
BlockExpression fullProgram = Expression.Block(
                new [ ] { i } , // Declare 'i' as a local variable within the block
                initializeI ,
                loop
            );
```

Step 10:

compile the expression and execute it.

```
Action compiledLoop = Expression.Lambda<Action>(fullProgram).Compile();
compiledLoop();
```

## examples
### exampl 1
Invoking following method

```
        /// <summary>
        /// illustrate how to create a loop using `Expression.Loop`.
        /// 
        /// The following example will output
        /// 
        /// ```
        /// i=0
        /// i=1
        /// i=2
        /// ```
        /// </summary>
        public static void TestMethod25()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            // 1. Define a ParameterExpression for our loop variable 'i'
            ParameterExpression i = Expression.Variable(typeof(int) , "i");

            // 2. Define a LabelTarget for the loop's break condition
            LabelTarget loopBreak = Expression.Label("loopBreak");

            // 3. Define the loop's initialization (i = 0)
            Expression initializeI = Expression.Assign(i , Expression.Constant(0));

            // 4. Define the loop body
            Expression loopBody = Expression.Block(
                // Print "i=" + i
                Expression.Call(
                    typeof(Console).GetMethod("WriteLine" , new [ ] { typeof(string) }) ,
                    Expression.Call(
                        typeof(string).GetMethod("Concat" , new [ ] { typeof(object) , typeof(object) }) , // Or other overloads as needed
                        Expression.Constant("i=") ,
                        Expression.Convert(i , typeof(object)) // Convert 'i' to object for string.Concat(object, object)
                    )
                ),
                // Postfix Increment to i
                Expression.PostIncrementAssign(i) ,
                // Check condition (i < 3) and break if false
                Expression.IfThen(
                    Expression.GreaterThanOrEqual(i , Expression.Constant(3)) ,
                    Expression.Break(loopBreak)
                )
            );

            // 5. Create the LoopExpression
            LoopExpression loop = Expression.Loop(
                loopBody ,
                loopBreak // Associate the break label with the loop
            );

            // 6. Combine initialization and the loop into a block expression
            BlockExpression fullProgram = Expression.Block(
                new [ ] { i } , // Declare 'i' as a local variable within the block
                initializeI ,
                loop
            );

            // 7. Compile the expression tree into an executable Action
            Action compiledLoop = Expression.Lambda<Action>(fullProgram).Compile();

            // 8. Execute the compiled delegate
            compiledLoop();
        }
```

will output following.

```
In TestMethod25 method call,
i=0
i=1
i=2
```
