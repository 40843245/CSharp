# CH1 -- intro
## objectives
You will know

+ what is `System.Linq.Expressions.Expression`

## CH1.1 -- what is `System.Linq.Expressions.Expression`
It is the base class that represent expression tree.

Let's look at its syntax:

```
public abstract class Expression
```

Although it is an abstract class, it provides lots of methods to create various expression used in `C#`, from following fundamental one expression about

+ constant
+ arithmetic operations
+ logical operations
+ bitwise operations
+ assignment operations
+ etc

even it can create a block with complex expressions.

After that, one can compile it and execute it.

## reference
### further reading
+ [`System.Linq.Expressions.Expression`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression?view=net-8.0)
