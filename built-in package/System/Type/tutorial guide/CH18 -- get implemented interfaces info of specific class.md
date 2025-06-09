# CH18 -- get implemented interfaces info of specific class
## objectives
You will learn how to

+ get implemented interfaces info of specific class

## CH18.1 -- get implemented interfaces info of specific class
### `GetInterfaces` instance method
To get implemented interfaces info of specific class,

just simply invoke `GetInterfaces` instance method.

## examples
### example 1
#### extension members

`HighOrderFunctionsExtensionmembers.Apply` static method is defined in `HighOrderFunctionsExtensionmembers` static class as follows.

`Apply.cs`

```
    public static partial class HighOrderFunctionsExtensionmembers
    {
        public static List<TFunctionResult> Apply<TElement, TFunctionResult>(
            this IEnumerable<TElement> enumerables ,
            Func<TElement , TFunctionResult> func
        )
        {
            List<TFunctionResult> list = new List<TFunctionResult>();
            foreach(var enumerable in enumerables)
            {
                var result = func.Invoke(enumerable);
                list.Add(result);
            }
            return list;
        }
    }
```

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get interfaces info of specific class.
        /// </summary>
        public static void TestMethod11()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            EmployeeRepository employeeRepository = new EmployeeRepository();
            Type type = employeeRepository.GetType();
            var interfaceTypes = type.GetInterfaces();

            List<string> textList = new List<string>();
            int counter = 0;
            string formattingString = "{0}th {1}:{2}";
            string text = string.Empty;

            textList = interfaceTypes.Apply<Type , string>(
                interfaceType =>
                {
                    var result = string.Empty;
                    if(interfaceType is Type nonNullInterfaceType)
                    {
                        result = string.Format(
                           formattingString ,
                           counter ,
                           "interfaceType.Name:" ,
                           nonNullInterfaceType.Name
                       );
                        counter++;
                        return result;
                    }
                    result = string.Format(
                            formattingString ,
                            counter ,
                            "interfaceType is" ,
                            "null"
                        );
                    counter++;
                    return result;
                }
            );

            text = string.Join(System.Environment.NewLine , textList);
            Console.WriteLine(type.Name);
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod11 method call
EmployeeRepository
0th interfaceType.Name::IGenericRepository`1
```

## reference
### API docs
+ [`Type.GetInterfaces Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getinterfaces?view=netframework-4.8.1)