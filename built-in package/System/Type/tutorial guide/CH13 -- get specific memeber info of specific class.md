# CH13 -- get specific member info of specific class
## objectives
You will learn how to

+ get specific member info of specific class

## CH13.1 -- get specific member info of specific class
### `GetMember` instance method
To get specific member info of specific class,

just simply invoking `GetMember` instance method.

`GetMember` instance method will return `MemberInfo[]`.

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
        /// illustrate how to get specific member info of specific class.
        /// </summary>
        public static void TestMethod16()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            string memberToFind = "FirstName";

            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var memberInfos = personNicoType.GetMember(memberToFind);
            List<string> textList = new List<string>();
            int counter = 0;
            string formattingString = "{0}th {1}:{2}";
            string text = string.Empty;

            if(memberInfos.Length > 0)
            {
                text = $"In `{personNicoType.Name}` type, there are {memberInfos.Length} members named `{memberToFind}`\n";

                textList = memberInfos.Apply<MemberInfo , string>(
                    memberInfo =>
                    {
                        var result = string.Format(
                               formattingString ,
                               counter ,
                               "memberInfo.MemberType:" ,
                               memberInfo.MemberType
                           );
                        counter++;
                        return result;
                    }
                );

                text += string.Join(System.Environment.NewLine , textList);
            }
            else
            {
                text = $"In `{personNicoType.Name}` type, there are no members named `{memberToFind}`";
            }
            Console.WriteLine(text);

            EmployeeRepository employeeRepository = new EmployeeRepository();

            Type employeeRepositoryType = employeeRepository.GetType();
            memberInfos = employeeRepositoryType.GetMember(memberToFind);
            textList = new List<string>();
            counter = 0;
            formattingString = "{0}th {1}:{2}";
            text = string.Empty;

            if(memberInfos.Length > 0)
            {
                text = $"In `{employeeRepositoryType.Name}` type, there are {memberInfos.Length} members named `{memberToFind}`\n";

                textList = memberInfos.Apply<MemberInfo , string>(
                    memberInfo =>
                    {
                        var result = string.Format(
                               formattingString ,
                               counter ,
                               "memberInfo.MemberType:" ,
                               memberInfo.MemberType
                           );
                        counter++;
                        return result;
                    }
                );

                text += string.Join(System.Environment.NewLine , textList);
            }
            else
            {
                text = $"In `{employeeRepositoryType.Name}` type, there are no members named `{memberToFind}`";
            }
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod16 method call
In `Person` type, there are 1 members named `FirstName`
0th memberInfo.MemberType::Property
In `EmployeeRepository` type, there are no members named `FirstName`
```

## reference
### API docs
+ [`Type.GetMember Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmember?view=netframework-4.8.1)