# CH2 -- get specific method info of current `Type`
## objectives
You will learn how to 

+ get specific method info of current `Type`.

## CH2.1 -- get specific method info of current `Type`
### `GetMethod` instance method of current `Type`
Invoking `GetMethod` instance method of current `Type` 

will return `MethodInfo?` representing specific method of current `Type`.

If there are no such methods defined in current `Type`,

it will return null (with `MethodInfo?` type).

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get specific method info of current type.
        /// </summary>
        public static void TestMethod2()
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
                substringMethodInfo ,
                3 ,
                2 ,
                result
            );
        }
```

will output following

```
In TestMethod2 method call
System.String Substring(Int32, Int32) with arguments 3 2 returns lo.
```

## reference
### API docs
+ [`MethodInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo?view=net-8.0)

+ [`Type.GetMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethod?view=net-8.0)