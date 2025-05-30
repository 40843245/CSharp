# CH2 -- get type
## objectives
You will learn how to 

+ get type of structure (such as `int`)
+ get type of an instance (such as `x` in `int x=2;`, `person` in `Person person = new Person();`)

**extra bonus**

+ get an array of type given an array of `object`

## CH2.1 -- get type of structure
### `typeof` function
To get type of structure, 

just simply invoking `typeof` function.

> [!CAUTION]
> Watch out the argument of `typeof` function.
> 
> The `typeof` function only accepts one arguments with `struct` (such as `typeof(int)`)

`typeof` function will return `System.Type` type.

In `System.Type` type, there is a getter property `FullName` that returns its full name.

We can compare a variable, and member is specific type as follows.

```
x.GetType() == typeof(int)
```

```
int x = 2;
if(x.GetType() == typeof(int))
{
    // x is `int` type
}
else
{
    // x is NOT `int` type
}
```

## CH2.2 -- get type of an instance
### `GetType` instance method
To get type of an instance,

just simply invoke `GetType` instance method

`GetType` instance method will return `System.Type` type.

```
class Person {}

Person person = new Person();
Type type = person.GetType();
```

There are lots of overloads, see [`Type.GetType Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.gettype?view=net-8.0)

## CH2.3 -- get an array of type given an array of `object`
### `Type.GetTypeArray` static method
In `Type` class under `System` namespace, it also defines a useful method.

+ `Type.GetTypeArray` static method: 

given an `object[]` instance as first argument, 

`Type.GetTypeArray` static method will return `Type[]`.

## examples
### example 1
#### code about extension methods
`TypeExtensionMethods.GetInfo` extension method is defined in `TypeExtensionMethods` static class as follows.

```
using Example.Helpers.Indentation;
using System;
using System.Text;

namespace Example.Extensions.ExtensionMethods.TypeExtensionMethods
{
    public static class TypeInfoExtensionMethods
    {
        public static IndentationHandler indentationHandler = new IndentationHandler(0,' ','+');
        public static bool HasBaseType(
            this Type type
        )
        {
            return type.BaseType != null;
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

we omitted `IndentationHandler` helper class and other code here.

For complete code, see project in [demo project](../demo%20project.md)

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate the use of `typeof` function.
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);
            
            //List<int> intList1 = new List<int>();

            Type [ ] datas = new Type [ ]
            {
                typeof(bool),
                typeof(byte),
                typeof(short),
                typeof(int),
                typeof(long),
                typeof(decimal),
                typeof(ushort),
                typeof(uint),
                typeof(ulong),
                typeof(char),
                typeof(string),
                typeof(List<int>),
                typeof(HashSet<int>),
                typeof(KeyValuePair<int,string>),
                typeof(Dictionary<int,string>),
                typeof(IList<int>),
                typeof(ICollection<int>),
                typeof(System.Console),
                typeof(Person),
                typeof(Employee),
                // typeof(intList1), // will throw compiler error.
            };

            foreach(var data in datas)
            {
                string infoText = data.GetInfo();
                Console.WriteLine(infoText);
            }
        }
```

will output following

```
In TestMethod1 method call
The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Boolean
 + FullName:System.Boolean
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Boolean, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Byte
 + FullName:System.Byte
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Byte, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Int16
 + FullName:System.Int16
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Int16, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Int32
 + FullName:System.Int32
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Int64
 + FullName:System.Int64
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Int64, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Decimal
 + FullName:System.Decimal
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Decimal, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:UInt16
 + FullName:System.UInt16
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.UInt16, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:UInt32
 + FullName:System.UInt32
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.UInt32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:UInt64
 + FullName:System.UInt64
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.UInt64, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Char
 + FullName:System.Char
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Char, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:String
 + FullName:System.String
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:List`1
 + FullName:System.Collections.Generic.List`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.List`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:HashSet`1
 + FullName:System.Collections.Generic.HashSet`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:System.Core, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.HashSet`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], System.Core, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:KeyValuePair`2
 + FullName:System.Collections.Generic.KeyValuePair`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.KeyValuePair`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Dictionary`2
 + FullName:System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:IList`1
 + FullName:System.Collections.Generic.IList`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.IList`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:ICollection`1
 + FullName:System.Collections.Generic.ICollection`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.ICollection`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Console
 + FullName:System.Console
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Console, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Person
 + FullName:Example.Bean.Person
 + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + AssemblyQualifiedName:Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Employee
 + FullName:Example.DirtyBean.Employee
 + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + AssemblyQualifiedName:Example.DirtyBean.Employee, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Person
  + FullName:Example.Bean.Person
  + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
  + AssemblyQualifiedName:Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

```

### example 2
#### extension methods

same above example 1.

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate get an array of type given `object[]` instance through `Type.GetTypeArray` static method.
        /// </summary>
        public static void TestMethod20()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int clubId = 1;
            string clubName = "LoveLive club";

            Person personNico = new Person()
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Employee employeeNico = new Employee()
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
                Company = new Company() 
                {
                    Id = clubId,
                    Name = clubName,
                }
            };

            List<object> objectList = new List<object>() 
            { 
                personNico,
                employeeNico
            };

            Dictionary<int , Person> personDictionary = new Dictionary<int , Person>();

            EmployeeRepository employeeRepository = new EmployeeRepository();

            IGenericRepository<Person> iPersonnelRepository = new GenericRepository<Person>();

            object[] objectArray = new object [ ]
            {
                clubId,
                clubName,
                objectList,
                personDictionary,
                personNico,
                employeeNico,
                employeeRepository,
                iPersonnelRepository,
                clubId.GetType(),
            };

            Type[] datas = Type.GetTypeArray(objectArray);
            
            foreach(var data in datas)
            {
                string infoText = data.GetInfo();
                Console.WriteLine(infoText);
            }
        }
```

will output following

```
In TestMethod20 method call
The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Int32
 + FullName:System.Int32
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:ValueType
  + FullName:System.ValueType
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.ValueType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:String
 + FullName:System.String
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:List`1
 + FullName:System.Collections.Generic.List`1[[System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.List`1[[System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Dictionary`2
 + FullName:System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]], mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Person
 + FullName:Example.Bean.Person
 + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + AssemblyQualifiedName:Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:Employee
 + FullName:Example.DirtyBean.Employee
 + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + AssemblyQualifiedName:Example.DirtyBean.Employee, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Person
  + FullName:Example.Bean.Person
  + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
  + AssemblyQualifiedName:Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:EmployeeRepository
 + FullName:Example.Repositories.EmployeeRepository
 + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + AssemblyQualifiedName:Example.Repositories.EmployeeRepository, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:GenericRepository`1
  + FullName:Example.Generics.Repository.GenericRepository`1[[Example.DirtyBean.Employee, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
  + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
  + AssemblyQualifiedName:Example.Generics.Repository.GenericRepository`1[[Example.DirtyBean.Employee, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]], Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:GenericRepository`1
 + FullName:Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
 + Assembly.FullName:Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + AssemblyQualifiedName:Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]], Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

The instance (is at IndentationLevel:0) with type `Type`:0
 + Name:RuntimeType
 + FullName:System.RuntimeType
 + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + AssemblyQualifiedName:System.RuntimeType, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
 + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:TypeInfo
  + FullName:System.Reflection.TypeInfo
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Reflection.TypeInfo, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Type
  + FullName:System.Type
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Type, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:MemberInfo
  + FullName:System.Reflection.MemberInfo
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Reflection.MemberInfo, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + There are base type as follows:
The instance (is at IndentationLevel:1) with type `Type`:1
  + Name:Object
  + FullName:System.Object
  + Assembly.FullName:mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
  + AssemblyQualifiedName:System.Object, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
There are no base type.
~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

~~~~ end at IndentationLevel:0~~~~

```
## reference
### API docs
+ [`Type Class`](https://learn.microsoft.com/en-us/dotnet/api/system.type?view=net-8.0)
+ [`Type.GetType Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.gettype?view=net-8.0)
+ [`Type.GetTypeArray(Object[]) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.gettypearray?view=net-8.0)

### docs
+ [`Type-testing operators and cast expressions - is, as, typeof, and casts`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/type-testing-and-cast)