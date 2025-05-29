# CH26 -- get all enum values in enum
## objectives
You will learn how to

+ get all enum values in enum

## CH26.1 -- get all enum values in enum
### `GetEnumValues` instance method
#### Overloads
```
public virtual Array GetEnumValues();
```

It will return an array of enum values7 in current enum.

If there are no members defined in current enum, it will return an empty array (`Array` type)

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to get all enum values in enum
        /// </summary>
        public static void TestMethod30()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type enumType;
            Array enumValues;
            string text;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(BindingFlags);

            enumValues = enumType.GetEnumValues();

            text = string.Join(System.Environment.NewLine , enumValues);

            Console.WriteLine("There are {0} members defined in current enum {1}" , enumValues.Length , enumType.FullName);
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            enumType = typeof(EmptyEnum);

            enumValues = enumType.GetEnumValues();

            text = string.Join(System.Environment.NewLine , enumValues);

            Console.WriteLine("There are {0} members defined in current enum {1}" , enumValues.Length , enumType.FullName);
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod30 method call
///* ------------------ Example 1 ------------------ *///
There are 20 members defined in current enum System.Reflection.BindingFlags
System.Reflection.BindingFlags[]

///* ------------------ Example 2 ------------------ *///
There are 0 members defined in current enum Example.Constants.Enums.EmptyEnum
Example.Constants.Enums.EmptyEnum[]

```

## reference
### API docs
+ [`Type.GetEnumValues Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getenumvalues?view=net-8.0)