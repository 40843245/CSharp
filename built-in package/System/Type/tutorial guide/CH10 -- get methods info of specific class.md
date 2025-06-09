# CH12 -- get methods info of specific type
## objectives
You will learn how to

+ get methods info of specific type

## CH12.1 -- get methods info of specific type
### `GetMethods` instance method
To get methods info of specific type,

just simply invoking `GetMethods` instance method.

`GetMethods` instance method will return `MethodInfo[]`.

## examples
### example 1
#### extension methods

`HighOrderFunctionsExtensionMethods.Apply` static method is defined in `HighOrderFunctionsExtensionMethods` static class as follows.

`Apply.cs`

```
    public static partial class HighOrderFunctionsExtensionMethods
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
        /// illustrate how to get methods info of specific type.
        /// </summary>
        public static void TestMethod9()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            EmployeeRepository employeeRepository = new EmployeeRepository();
            Type type = employeeRepository.GetType();
            var methodInfos = type.GetMethods();

            List<string> textList = new List<string>();
            int counter = 0;
            string formattingString = "{0}th {1}:{2}";
            string text = string.Empty;

            textList = methodInfos.Apply<MethodInfo,string>(
                methodInfo =>
                {
                    var result = string.Empty;
                    if(methodInfo is MethodInfo nonNullMethodInfo)
                    {
                        result = string.Format(
                           formattingString ,
                           counter ,
                           "methodInfo.Name:" ,
                           nonNullMethodInfo.Name
                       );
                        counter++;
                        return result;
                    }
                    result = string.Format(
                            formattingString ,
                            counter ,
                            "methodInfo is" ,
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
In TestMethod9 method call
EmployeeRepository
0th methodInfo.Name::DeleteOne
1th methodInfo.Name::GetFirstOrNull
2th methodInfo.Name::InsertOne
3th methodInfo.Name::UpdateOne
4th methodInfo.Name::Equals
5th methodInfo.Name::GetHashCode
6th methodInfo.Name::GetType
7th methodInfo.Name::ToString
```

## reference
### API docs
+ [`Type.GetMethods Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethods?view=netframework-4.8.1)