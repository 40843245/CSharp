# CH5 -- generic method
## objectives
You will learn how to

+ make a method into a generic method

+ get the method definition from a generic method

+ get the type of arguments of a generic method or the type parameters of a generic method definition

## CH5.1 -- make a method into a generic method
### `MakeGenericMethod` instance method
Given current (non-generic) method info (whose method is defined with generic) and `Type`,

`MakeGenericMethod` instance method will return the current generic method info.

## CH5.2 -- get the method definition from a generic method
### `GetGenericMethodDefinition` instance method
Given the current generic method info, 

`GetGenericMethodDefinition` instance method will return the (non-generic) method definition.

## CH5.3 -- get the type of arguments of a generic method or the type parameters of a generic method definition
### `GetGenericArguments` instance method
`GetGenericArguments` instance method will get the type of arguments of a generic method or the type parameters of a generic method definition.

It will return `Type[]`.

If there are no arguments of a generic method or the type parameters,

it will return an empty array of `Type`.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to 
        /// 
        /// + make a method into a generic method
        /// + get the method definition from a generic method
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string text = string.Empty;
            Type ex = typeof(OutputHandler);
            MethodInfo mi = ex.GetMethod("Generic");

            text = mi.GetGenericMethodInfo();
            Console.WriteLine(text);

            MethodInfo miConstructed = mi.MakeGenericMethod(typeof(int));

            text = miConstructed.GetGenericMethodInfo();
            Console.WriteLine(text);

            MethodInfo miDef = miConstructed.GetGenericMethodDefinition();
            Console.WriteLine("\r\nThe definition is the same: {0}" ,
                miDef == mi);

            text = miDef.GetGenericMethodInfo();
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod5 method call,

Void Generic[T](T)
        Is this a generic method definition? True
        Is it a generic method? True
        Does it have unassigned generic parameters? True
        List type arguments (1):
                T  parameter position 0
                   declaring method: Void Generic[T](T)


Void Generic[Int32](Int32)
        Is this a generic method definition? False
        Is it a generic method? True
        Does it have unassigned generic parameters? False
        List type arguments (1):
                System.Int32


The definition is the same: True

Void Generic[T](T)
        Is this a generic method definition? True
        Is it a generic method? True
        Does it have unassigned generic parameters? True
        List type arguments (1):
                T  parameter position 0
                   declaring method: Void Generic[T](T)
```

## reference
### API docs
+ [`MethodInfo.MakeGenericMethod(Type[]) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.makegenericmethod?view=net-8.0) 

+ [`MethodInfo.GetGenericMethodDefinition Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.getgenericmethoddefinition?view=net-8.0)

+ [`MethodInfo.GetGenericArguments Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.getgenericarguments?view=net-8.0)

### examples
+ The code of example 1 is referenced from example of [`MethodInfo.MakeGenericMethod(Type[]) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.makegenericmethod?view=net-8.0) and [`MethodInfo.GetGenericMethodDefinition Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.getgenericmethoddefinition?view=net-8.0)