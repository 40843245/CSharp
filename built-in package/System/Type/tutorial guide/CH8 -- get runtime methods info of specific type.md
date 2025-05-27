# CH8 -- get runtime methods info of specific type
## objectives
You will learn how to

+ get runtime methods info of specific type

## CH8.1 -- get runtime methods info of specific type
### `GetRuntimeMethods` instance method
To get runtime methods info of specific type,

just simply invoking `GetRuntimeMethods` instance method.

`GetRuntimeMethods` instance method will return `IEnumerable<MethodInfo>`.

Syntax:

```
        //
        // 摘要:
        //     擷取集合，表示指定的型別所定義的所有方法。
        //
        // 參數:
        //   type:
        //     包含方法的型別。
        //
        // 傳回:
        //     所指定型別的方法集合。
        //
        // 例外狀況:
        //   T:System.ArgumentNullException:
        //     type 為 null。
        public static IEnumerable<MethodInfo> GetRuntimeMethods(this Type type);
```
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
        /// illustrate how to get runtime methods' info.
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            EmployeeRepository employeeRepository = new EmployeeRepository();
            Type type = employeeRepository.GetType();
            var methodInfos = type.GetRuntimeMethods();

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
                            "methodInfo.Name:",
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

            text = string.Join(System.Environment.NewLine ,textList);
            Console.WriteLine(type.Name);
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod8 method call
EmployeeRepository
0th methodInfo.Name::DeleteOne
1th methodInfo.Name::GetFirstOrNull
2th methodInfo.Name::InsertOne
3th methodInfo.Name::UpdateOne
4th methodInfo.Name::Equals
5th methodInfo.Name::GetHashCode
6th methodInfo.Name::Finalize
7th methodInfo.Name::GetType
8th methodInfo.Name::MemberwiseClone
9th methodInfo.Name::ToString
```

## reference
### API docs
None