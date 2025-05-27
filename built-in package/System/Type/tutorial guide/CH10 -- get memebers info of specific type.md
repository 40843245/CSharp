# CH9 -- get members info of specific type
## objectives
You will learn how to

+ get members info of specific type

## CH9.1 -- get members info of specific type
### `GetMembers` instance method
To get members info of specific type,

just simply invoking `GetMembers` instance method.

`GetMembers` instance method will return `MemberInfo[]`.

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
        /// illustrate how to get members info of specific type.
        /// </summary>
        public static void TestMethod10()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var memberInfos = personNicoType.GetMembers();

            List<string> textList = new List<string>();
            int counter = 0;
            string formattingString = "{0}th {1}:{2}";
            string text = string.Empty;

            textList = memberInfos.Apply<MemberInfo , string>(
                memberInfo =>
                {
                    var result = string.Empty;
                    if(memberInfo is MethodInfo nonNullMemberInfo)
                    {
                        result = string.Format(
                           formattingString ,
                           counter ,
                           "memberInfo.Name:" ,
                           nonNullMemberInfo.Name
                       );
                        counter++;
                        return result;
                    }
                    result = string.Format(
                            formattingString ,
                            counter ,
                            "memberInfo is" ,
                            "null"
                        );
                    counter++;
                    return result;
                }
            );

            text = string.Join(System.Environment.NewLine , textList);
            Console.WriteLine(personNicoType.Name);
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod10 method call
Person
0th memberInfo.Name::get_FirstName
1th memberInfo.Name::set_FirstName
2th memberInfo.Name::get_LastName
3th memberInfo.Name::set_LastName
4th memberInfo.Name::Equals
5th memberInfo.Name::GetHashCode
6th memberInfo.Name::GetType
7th memberInfo.Name::ToString
8th memberInfo is:null
9th memberInfo is:null
10th memberInfo is:null
```

## reference
### API docs
+ [`Type.GetMembers Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmembers?view=netframework-4.8.1)