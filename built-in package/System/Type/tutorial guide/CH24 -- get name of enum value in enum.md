# CH24 -- get name of enum value in enum
## objectives
You will learn how to

+ get name of enum value in enum

## CH24.1 -- get name of enum value in enum
### `GetEnum` instance method
Given value of enum, `GetEnum` instance method can return its enum name

Think of this,

Take `BindingFlags.Default` in `BindingFlags` enum for example,

`BindingFlags` enum is defined as follows.

```
using System.Runtime.InteropServices;

namespace System.Reflection
{
    // .. omit annotaions for clarity
    public enum BindingFlags
    {
        Default = 0,
        IgnoreCase = 1,
        DeclaredOnly = 2,
        Instance = 4,
        Static = 8,
        Public = 16,
        NonPublic = 32,
        FlattenHierarchy = 64,
        InvokeMethod = 256,
        CreateInstance = 512,
        GetField = 1024,
        SetField = 2048,
        GetProperty = 4096,
        SetProperty = 8192,
        PutDispProperty = 16384,
        PutRefDispProperty = 32768,
        ExactBinding = 65536,
        SuppressChangeType = 131072,
        OptionalParamBinding = 262144,
        IgnoreReturn = 16777216
    }
}
```

In aboe definition, we can know that

+ it defines a `Default` name  with value `0` (in `int` type) 

which is equal to the number converting from `BindingFlags.Default` (in `BindingFlags` type) to `int`. 

```
(int)`BindingFlags.Default` // 0
```

```
    // name = value
    Default = 0,
```

Thus, executing

```
typeof(BindingFlags).GetEnumName(BindingFlags.Default)
```

and

```
typeof(BindingFlags).GetEnumName(0)
```

will BOTH return `Default` (with string type).

#### Overloads
```
public virtual string? GetEnumName(object value);
```

#### Exceptions

+ If current type is NOT enum type, it will throw `ArgumentException`.

For example,

executing following statement will throw `ArgumentException`

```
typeof(int).GetEnumValue(int.MaxValue); // throw `ArgumentException`
```

+ If the argument `value` (that represents the target to be searched for) is `null`, it will throw `ArgumentNullException`.

For example,

executing following statement will throw `ArgumentNullException`

```
typeof(BindingFlags).GetEnumValue(null); // throw `ArgumentNullException`
```

+ If the argument `value` (that represents the target to be searched for) is not defined in current enum, it throw `ArgumentException`

For example,

executing following statement will throw `ArgumentException`

```
typeof(BindingFlags).GetEnumValue(3); // throw `ArgumentException`
```

since there are NO any enum value in `BindingFlags` is defined as `3`.

#### Remarks
> [!IMPORTANT]
> When type of enum value is NOT same as current enum type,
>
> it will NOT throw exceptions.
>
> since the enum value will be converted as `object` type when passed with argument according to its overloads.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get name with an enum value.
        /// </summary>
        public static void TestMethod27()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type enumType;
            string enumName = string.Empty;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(BindingFlags);
            enumName = enumType.GetEnumName(BindingFlags.Public);

            Console.WriteLine("The enum name in enum {0} is {1}" , enumType.FullName, enumName);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(BindingFlags);
            enumName = enumType.GetEnumName(16); // value of `Binding.Public` in `int` type.

            Console.WriteLine("The enum name in enum {0} is {1}" , enumType.FullName , enumName);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 3 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(MemberTypes);
            enumName = enumType.GetEnumName(MemberTypes.Property);

            Console.WriteLine("The enum name in enum {0} is {1}" , enumType.FullName,enumName);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 4 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(MemberTypes);
            enumName = enumType.GetEnumName(16); // value of `MemberTypes.Property` in `int` type. 

            Console.WriteLine("The enum name in enum {0} is {1}" , enumType.FullName , enumName);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod27 method call
///* ------------------ Example 1 ------------------ *///
The enum name in enum System.Reflection.BindingFlags is Public

///* ------------------ Example 2 ------------------ *///
The enum name in enum System.Reflection.BindingFlags is Public

///* ------------------ Example 3 ------------------ *///
The enum name in enum System.Reflection.MemberTypes is Property

///* ------------------ Example 4 ------------------ *///
The enum name in enum System.Reflection.MemberTypes is Property

```

### example 2
#### main code
Invoking following method will NOT throw an exception, 

even if the type of enum value is NOT same as current enum type.

```
        /// <summary>
        /// illustrate how to get name with an enum value.
        /// </summary>
        public static void TestMethod28()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type enumType;
            string enumName = string.Empty;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(BindingFlags);
            enumName = enumType.GetEnumName(MemberTypes.Property);

            Console.WriteLine("The enum name in enum {0} is {1}" , enumType.FullName , enumName);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(BindingFlags);
            enumName = enumType.GetEnumName((MemberTypes)16);

            Console.WriteLine("The enum name in enum {0} is {1}" , enumType.FullName , enumName);
            Console.WriteLine();

            counter++;
        }
```

It will output following

```
In TestMethod28 method call
///* ------------------ Example 1 ------------------ *///
The enum name in enum System.Reflection.BindingFlags is Public

///* ------------------ Example 2 ------------------ *///
The enum name in enum System.Reflection.BindingFlags is Public

```

## reference
### API docs
+ [`Type.GetEnumName(Object) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getenumname?view=net-8.0)