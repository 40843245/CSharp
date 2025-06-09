# CH9 -- get properties info of specific type
## objectives
You will learn how to

+ get properties info of specific type

## CH9.1 -- get properties info of specific type
### `GetProperties` instance method
To get properties info of specific type,

just simply invoking `GetProperties` instance method.

`GetProperties` instance method will return `PropertyInfo[]`.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get properties info of specific class.
        /// </summary>
        public static void TestMethod13()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);
            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var propertyInfos = personNicoType.GetProperties();

            List<string> textList = new List<string>();
            int counter = 0;
            string formattingString = "{0}th {1}:{2}";
            string text = string.Empty;

            textList = propertyInfos.Apply<PropertyInfo , string>(
                propertyInfo =>
                {
                    var result = string.Empty;
                    if(propertyInfo is PropertyInfo nonNullPropertyInfo)
                    {
                        result = string.Format(
                           formattingString ,
                           counter ,
                           "propertyInfo.Name:" ,
                           nonNullPropertyInfo.Name
                       );
                        counter++;
                        return result;
                    }
                    result = string.Format(
                            formattingString ,
                            counter ,
                            "propertyInfo is" ,
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
In TestMethod13 method call
Person
0th propertyInfo.Name::FirstName
1th propertyInfo.Name::LastName
```

## reference
### API docs
+ [`Type.GetProperties Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getproperties?view=netframework-4.8.1)