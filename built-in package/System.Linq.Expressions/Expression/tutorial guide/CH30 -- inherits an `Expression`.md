# CH30 -- inherits an `Expression`
## objectives
You will learn how to

+ inherits an `Expression`

## CH30.1 -- inherits an `Expression`
Step 1:

define a class that inherits `System.Linq.Expressions.Expression` class 

and then implements its methods.

These method MUST be implemented.

+ `Reduce` (since its defined with `virtual` keyword)

Basic structure looks like following:

```
public class PostIncrementExpression : Expression
{
    //...

        public override ExpressionType NodeType => Variable.NodeType; 
        public override Type Type => Variable.Type;

        public override Expression Reduce()
        {
            return //... Expression
        }
}
```

Step 2:

Define a getter property with type `Expression` that used in `Reduce` method

since following reasons

+ the `Reduce` method MUST return `Expression`
+ there are no parameter in `Reduce` method

Thus, if we want the `Reduce` method does NOT always return same expression,

we need to use at least one properties (in the derived class).

The easiest to do that is

1. define a getter property with type `Expression`.
2. initialize it in constructor.

Step 3:

override custom getter property (of course, that is in `Expression` class).

The getter properties in `Expression` class has

+ `NodeType`
+ `Type`
+ `CanReduce`

For more details, see [`Expression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression?view=net-8.0)

One usually overrides the getter properties in `Expression` class, 

One usually overrides the behavior of these getter property according the getter property with `Expression` (in the derived class). 

See following example for more details.

## example
### example 1
Invoking this method

```
        /// <summary>
        /// illustrate how to inherit `Expression` and implement `Expression.Reduce` virtual method.
        /// </summary>
        public static void TestMethod42()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            
            ParameterExpression x = Expression.Parameter(typeof(int) , "x");

            PostIncrementExpression postIncrementX = new PostIncrementExpression(x);

            var compiledX = Expression.Lambda<Func<int , int>>(x,x).Compile();

            //To create an lambda expression by `Expression.Lambda`, the expression MUST be reduced.
            // this will throw exception. `postIncrementX` is NOT reduced.
            //var compilePostIncrementX = Expression.Lambda<Func<int , int>>(postIncrementX,x).Compile();

            var compileReducedPostIncrementX = Expression.Lambda<Func<int , int>>(postIncrementX.Reduce(),x).Compile();

            Console.WriteLine("compiledX:{0}" , compiledX.ToString());
            Console.WriteLine("compileReducedPostIncrementX:{0}" , compileReducedPostIncrementX.ToString());
        }
```

where

`PostIncrementExpression` class inherits `Expression` class defined as follows.

```
using System;
using System.Linq.Expressions;

namespace Example.Utilities.Expressions
{
    public class PostIncrementExpression : Expression
    {
        public new ParameterExpression Variable { get; }

        public PostIncrementExpression(ParameterExpression variable)
        {
            Variable = variable;
        }

        public override ExpressionType NodeType => Variable.NodeType; 
        public override Type Type => Variable.Type;

        public override bool CanReduce => Variable.CanReduce;

        public override Expression Reduce()
        {
            // 1. Create a temporary variable to store the original value
            ParameterExpression temp = Expression.Variable(Variable.Type , "temp");

            // 2. Create the block expression
            return Expression.Block(
                new [ ] { temp } , // Declare the temporary variable
                Expression.Assign(temp , Variable) , // Assign current 'x' to 'temp'
                Expression.Assign(Variable , Expression.Add(Variable , Expression.Constant(1))) , // x = x + 1
                temp // The result of the expression is the original value of x (stored in temp)
            );
        }
    }
}
```

> [!TIP]
> The reason why `new` keyword at the getter property 
> 
> ```
> public new ParameterExpression Variable { get; }
> ```
>
> is that there is `Expression.Variable` static method and 
>
> we want to hide `Expression.Variable` in derived class (according to `C#` syntax).
>
>[hide member in parent class.png](./hide%20member%20in%20parent%20class.png)

It will output following

```
In TestMethod42 method call,
compiledX:System.Func`2[System.Int32,System.Int32]
compileReducedPostIncrementX:System.Func`2[System.Int32,System.Int32]
```

## reference
### API docs
+ [`Expression.Reduce Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.reduce?view=net-8.0)
+ [`Expression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression?view=net-8.0)

### examples
+ The code in example 1 are generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/64f8454fd1e1) and then fixed by it.