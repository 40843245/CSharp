# CH16 -- create an expression containing a member
## objectives
You will learn how to

+ create an expression containing a member

## CH16.1 -- create an expression containing a member
### `Expression.Field` static method
You can create an expression containing a field by invoking `Expression.Field` static method.

### `Expression.Property` static method
You can create an expression containing a property by invoking `Expression.Property` static method.

> [!IMPORTANT]
> What is the difference between `fields` and `properties`, see [difference between `fields` and `properties`](https://g.co/gemini/share/7461cadebf80)

## examples
### example 1
Invoking the following method

```
        /// <summary>
        /// illustrate how to create an expression containing a member.
        /// </summary>
        public static void TestMethod18()
        {
            Animal horse = new Animal();

            // Create a MemberExpression that represents getting
            // the value of the 'species' field of class 'Animal'.
            MemberExpression memberExpression =
                Expression.Field(
                    Expression.Constant(horse) ,
                    "species");

            Console.WriteLine(memberExpression.ToString());
        }
```

where

`Animal` class is defined as follows.

`Animal.cs`

```
    public class Animal
    {
        public string species;
    }
```

It will output following

```
In TestMethod18 method call,
value(Example.AppData.Animal).species
```

## reference
### API docs
+ [`Expression.Field Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.field?view=net-8.0)
+ [`Expression.Property Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.property?view=net-8.0)
+ [`MemberExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.memberexpression?view=net-8.0)
