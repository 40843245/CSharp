# CH9 -- get fields info of specific class
## objectives
You will learn how to

+ get fields info of specific class

## CH9.1 -- get fields info of specific class
### `GetFields` instance method
To get fields info of specific class,

just simply invoking `GetFields` instance method.

`GetFields` instance method will return `FieldInfo[]`.

## examples
### example 1
#### main code
Invoking following method

```
       /// <summary>
        /// illustrate how to get fields info of specific class.
        /// </summary>
        public static void TestMethod12()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var fieldInfos = personNicoType.GetFields();

            List<string> textList = new List<string>();
            int counter = 0;
            string formattingString = "{0}th {1}:{2}";
            string text = string.Empty;

            textList = fieldInfos.Apply<FieldInfo , string>(
                fieldInfo =>
                {
                    var result = string.Empty;
                    if(fieldInfo is FieldInfo nonNullFieldInfo)
                    {
                        result = string.Format(
                           formattingString ,
                           counter ,
                           "fieldInfo.Name:" ,
                           nonNullFieldInfo.Name
                       );
                        counter++;
                        return result;
                    }
                    result = string.Format(
                            formattingString ,
                            counter ,
                            "fieldInfo is" ,
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
In TestMethod12 method call
Person

```

## reference
### API docs
+ [`Type.GetFields Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmembers?view=netframework-4.8.1)