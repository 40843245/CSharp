# CH2 -- get type
## objectives
You will learn how to 

+ get type of structure (such as `int`)
+ get type of a variable, or property (such as `x` in `int x=2;`)

## CH2.1 -- get type of structure
### `typeof` function
To get type of structure, 

just simply invoking `typeof` function.

`typeof` function will return `System.Type` type.

In `System.Type` type, there is a getter property `FullName` that returns its full name.

We can compare a variable, and member is specific type as follows.

```
x == typeof(int)
```

```
int x = 2;
if(x == typeof(int))
{
    // x is `int` type
}
else
{
    // x is NOT `int` type
}
```

## examples
### example 1
#### code about extension methods
`TypeExtensionMethods.GetInfo` and `TypeExtensionMethods.GetSimpleInfo` extension method is defined in `TypeExtensionMethods` static class as follows.

```
using Example.Helpers.Indentation;
using System;
using System.Text;

namespace Example.Extensions.ExtensionMethods.TypeExtensionMethods
{
    public static class TypeExtensionMethods
    {
        public static IndentationHandler indentationHandler = new IndentationHandler(0,' ','+');
        public static bool HasBaseType(
            this Type type
        )
        {
            return type.BaseType != null;
        }
        public static string GetSimpleInfo(
            this Type data,
            int indentationLevel = 0
        )
        {
            indentationHandler.IndentationLevel = indentationLevel;

            StringBuilder stringBuilder = new StringBuilder();
            
            stringBuilder.AppendFormat("The instance (is at IndentationLevel:{0}) with type `Type`:{0}\n" , indentationLevel, data.ToString());
            stringBuilder.AppendLine(indentationHandler.GetIndentedMessage(data.Name));
            stringBuilder.AppendLine(indentationHandler.GetIndentedMessage(data.FullName));
            stringBuilder.AppendLine(indentationHandler.GetIndentedMessage(data.Assembly.FullName));
            stringBuilder.AppendLine(indentationHandler.GetIndentedMessage(data.AssemblyQualifiedName));

            if(data.HasBaseType())
            {
                stringBuilder.AppendLine(indentationHandler.GetIndentedMessage("There are base type as follows:"));
                stringBuilder.AppendLine(data.BaseType.GetSimpleInfo(indentationLevel+1));
            }
            
            stringBuilder.AppendFormat("~~~~ end at IndentationLevel:{0}~~~~\n", indentationLevel);
            return stringBuilder.ToString();
        }
        public static string GetInfo(
            this Type data,
            int indentationLevel = 0
        )
        {
            indentationHandler.IndentationLevel = indentationLevel;

            StringBuilder stringBuilder = new StringBuilder();

            stringBuilder.AppendFormat("The instance (is at IndentationLevel:{0}) with type `Type`:{0}\n" , indentationLevel , data.ToString());
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("Name:{0}\n") , data.Name);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("FullName:{0}\n"), data.FullName);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("Assembly.FullName:{0}\n") , data.Assembly.FullName);
            stringBuilder.AppendFormat(indentationHandler.GetIndentedMessage("AssemblyQualifiedName:{0}\n") , data.AssemblyQualifiedName);
            if(data.HasBaseType())
            {
                stringBuilder.AppendLine(indentationHandler.GetIndentedMessage("There are base type as follows:"));
                stringBuilder.AppendLine(data.BaseType.GetInfo(1));
            }
            else
            {
                stringBuilder.AppendLine("There are no base type.");
            }

            stringBuilder.AppendFormat("~~~~ end at IndentationLevel:{0}~~~~\n" , 0);
            return stringBuilder.ToString();
        }
    }
}
```

we omitted `IndentationHandler` here.

For complete code, see project in [demo project](../demo%20project.md)