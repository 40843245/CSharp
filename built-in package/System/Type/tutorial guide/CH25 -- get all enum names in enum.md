# CH25 -- get all enum names in enum
## objectives
You will learn how to

+ get all enum names in enum

## CH25.1 -- get all enum names in enum
### `GetEnumNames` instance method
#### Overloads
```
public virtual string[] GetEnumNames();
```

It will return an array of enum name in current enum.

If there are no members defined in current enum, it will return an empty array of string.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to get all enum names in enum
        /// </summary>
        public static void TestMethod29()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type enumType;
            string[] enumNames;
            string text;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(BindingFlags);

            enumNames = enumType.GetEnumNames();

            text = string.Join(System.Environment.NewLine , enumNames);

            Console.WriteLine("There are {0} members defined in current enum {1}",enumNames.Length,enumType.FullName);
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(EmptyEnum);

            enumNames = enumType.GetEnumNames();

            text = string.Join(System.Environment.NewLine , enumNames);

            Console.WriteLine("There are {0} members defined in current enum {1}" , enumNames.Length , enumType.FullName);
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod29 method call
///* ------------------ Example 1 ------------------ *///
There are 20 members defined in current enum System.Reflection.BindingFlags
Default
IgnoreCase
DeclaredOnly
Instance
Static
Public
NonPublic
FlattenHierarchy
InvokeMethod
CreateInstance
GetField
SetField
GetProperty
SetProperty
PutDispProperty
PutRefDispProperty
ExactBinding
SuppressChangeType
OptionalParamBinding
IgnoreReturn

///* ------------------ Example 2 ------------------ *///
There are 0 members defined in current enum Example.Constants.Enums.EmptyEnum


```

## reference
### API docs
+ [`Type.GetEnumNames Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getenumnames?view=net-8.0)