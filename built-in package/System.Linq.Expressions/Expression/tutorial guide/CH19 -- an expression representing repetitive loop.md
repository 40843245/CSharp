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

```

Step 4:

To create an expression containing a condition, for example, when the `i>3` condition is true, then exits the repetitive loop,

we need to invoke `Expression.`

```
```


