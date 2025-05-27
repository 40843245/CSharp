# CH7 -- get method info of specific type
## objectives
You will learn how to

+ get method info of specific type

## CH7.1-- get method info of specific type
### `GetMethod` instance method
It will return `MethodInfo` type (representing the info of method)

by invoking `GetMethod` instance method.

## examples
### example 1
This example illustrates how to get info `SubString` instance method of string instance.

Then invoke `"Hello World".SubString(3,2)`;

Invoking following method

```
        /// <summary>
        /// illustrate how to get method info of specific type.
        /// </summary>
        public static void TestMethod7()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Type stringType = typeof(string);
            MethodInfo substringMethodInfo =
                stringType.GetMethod(
                    "Substring" ,
                    new Type [ ] { typeof(int) , typeof(int) }
                );

            string message = "Hello World";
            object result = substringMethodInfo.Invoke(message , new object [ ] { 3 , 2 });
            Console.WriteLine(
                "{0} with arguments {1} {2} returns {3}." ,
                substringMethodInfo,
                3,
                2,
                result
            );
        }
```

will output following

```
In TestMethod7 method call
System.String Substring(Int32, Int32) with arguments 3 2 returns lo.
```

## reference
### API docs
+ [`Type Class`](https://learn.microsoft.com/en-us/dotnet/api/system.type?view=net-9.0)
+ [`Type.GetMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethod?view=net-9.0)

### examples
+ The code in example 1 are modified from an example in [`Type Class`](https://learn.microsoft.com/en-us/dotnet/api/system.type?view=net-9.0)