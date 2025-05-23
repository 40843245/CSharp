# CH16 -- an expression representing unconditional jump
## objectives
You will learn how to 

+ create an expression about unconditional jump

## CH16.1 -- create an expression representing unconditional jump
### `Expression.Goto` static method
To create an expression representing unconditional jump, 

just simply invoke `Expression.Goto` static method.

Think of this:

After compiling the code snippets,

```
LabelTarget returnTarget = Expression.Label();
BlockExpression blockExpr =
    Expression.Block(
        Expression.Call(typeof(Console).GetMethod("WriteLine", new Type[] { typeof(string) }), Expression.Constant("GoTo")),
        Expression.Goto(returnTarget),
        Expression.Call(typeof(Console).GetMethod("WriteLine", new Type[] { typeof(string) }), Expression.Constant("Other Work")),
        Expression.Label(returnTarget)
    );
```

it will become

```
Console.WriteLine("GoTo");
goto @unknown:
Console.WriteLine("Other Work");
label @unknown
```

where 

the label has no name (but for easier to discuss it, the author uses `@unknown` which indicates the label's name is unknown.)

When execute it,

it will only output

```
GoTo
```

since after executing this statement `Console.WriteLine("GoTo");`, it will jump to the end.

## examples
### example 1
Invoking the following method

```
        /// <summary>
        /// illustrate how to create an expression representing unconditional jump (with `label` and `goto` keyword.
        /// </summary>
        public static void TestMethod24()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            // A label expression of the void type that is the target for Expression.Return().
            LabelTarget returnTarget = Expression.Label();

            // This block contains a GotoExpression that represents a return statement with no value.
            // It transfers execution to a label expression that is initialized with the same LabelTarget as the GotoExpression.
            // The types of the GotoExpression, label expression, and LabelTarget must match.
            BlockExpression blockExpression =
                Expression.Block(
                    Expression.Call(typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) }) , Expression.Constant("Return")) ,
                    Expression.Return(returnTarget) ,
                    Expression.Call(typeof(Console).GetMethod("WriteLine" , new Type [ ] { typeof(string) }) , Expression.Constant("Other Work")) ,
                    Expression.Label(returnTarget)
                );

            // The following statement first creates an expression tree,
            // then compiles it, and then runs it.
            Expression.Lambda<Action>(blockExpression).Compile()();
        }
```

will output following

```
In TestMethod24 method call,
Return
```

## reference
### API docs
+ [`Expression.Label Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.label?view=net-8.0)
+ [`Expression.Goto Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.goto?view=net-8.0)
+ [`LabelTarget Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.labeltarget?view=net-8.0)
+ [`GotoExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.gotoexpression?view=net-8.0)
