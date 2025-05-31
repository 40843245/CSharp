# CH1 -- get all methods info of current `Type`
## objectives
You will learn how to 

+ get all methods info of current `Type`.

## CH1.1 -- get all methods info of current `Type`
### `GetMethod` instance method of current `Type`
Invoking `GetMethod` instance method of current `Type` 

will return `MethodInfo[]` representing all methods of the instance.

If there are no methods defined in current `Type`,

it will return empty array of `MethodInfo`.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// get all methods info of specific current `Type`.
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call,",MethodBase.GetCurrentMethod().Name);

            Type [ ] types = new Type[]
            {
                typeof(GenericRepository<Company>),
                typeof(IGenericRepository<Company>),

            };

            for(int i=0;i<types.Length;i++) 
            {
                Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , i+1);
                
                Type type = types [ i ];
                MethodInfo [ ] methodInfos = type.GetMethods();

                List<string> textList = new List<string>();
                int counter = 0;
                string formattingString = "{0}th {1}:{2}";
                string text = string.Empty;

                textList = methodInfos.Apply<MethodInfo , string>(
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

                Console.WriteLine();
            }
        }
```

will output following

```
In TestMethod1 method call,
///* --------------- Example 1 --------------- *///
GenericRepository`1
0th methodInfo.Name::DeleteOne
1th methodInfo.Name::GetFirstOrNull
2th methodInfo.Name::InsertOne
3th methodInfo.Name::UpdateOne
4th methodInfo.Name::Equals
5th methodInfo.Name::GetHashCode
6th methodInfo.Name::GetType
7th methodInfo.Name::ToString

///* --------------- Example 2 --------------- *///
IGenericRepository`1
0th methodInfo.Name::GetFirstOrNull
1th methodInfo.Name::InsertOne
2th methodInfo.Name::DeleteOne
3th methodInfo.Name::UpdateOne

```

## reference
### API docs
+ [`MethodInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo?view=net-8.0)

+ [`Type.GetMethods Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethods?view=net-8.0)