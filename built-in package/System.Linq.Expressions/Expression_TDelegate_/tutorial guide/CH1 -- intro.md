# CH1 -- intro
## objectives
You will know 

+ what can `System.Linq.Expressions.Expression<TDelegate> Class` do
+ class hierarchy
+ syntax of class

## CH1.1 -- what can `System.Linq.Expressions.Expression<TDelegate> Class` do
It can represents a strongly typed lambda expression as a data structure in the form of an expression tree.

## CH1.2 -- class hierarchy
<img width="437" alt="image" src="https://github.com/user-attachments/assets/9cd1c222-0d3b-4055-a08c-da17269ddb60" />

> [!IMPORTANT]
> DON'T be confused with `System.Linq.Expressions.Expression<TDelegate>` and `System.Linq.Expressions.Expression`.
>
> In former one, a strongly type MUST be specified as its generic type.
>
> While, in latter one, there is NOT generic type.

## CH1.3 -- syntax of class
syntax of class as follows

```
public sealed class Expression<TDelegate> : System.Linq.Expressions.LambdaExpression
```

With `sealed` modifier, it can't be inherited.

## reference
### API docs
+ [`System.Linq.Expressions.Expression<TDelegate> Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression-1?view=net-8.0)
