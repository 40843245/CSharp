# CH5 -- get raw value of an property by a compiler
## objectives
You will learn how to

+ get raw value of an property by a compiler

## CH5.1 -- get raw value of an property by a compiler
### `GetRawConstantValue`

get a literal value associated with the property by a compiler.

> [!IMPORTANT]
> For a getter property, since it fetchs the value at runtime and 
> 
> its value is determined at runtime NOT at compile time 
>
> (it's one of feature of getter-property),
>
> it will throw an exception at runtime with message `Literal value was NOT found` 

#### Remarks
see [`PropertyInfo.GetRawConstantValue Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getrawconstantvalue?view=net-9.0)

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// get a literal value associated with the property by a compiler.
        /// 
        /// > [!IMPORTANT]
        /// > For a getter property, since it fetchs the value at runtime and 
        /// > 
        /// > its value is determined at runtime NOT at compile time 
        /// >
        /// > (it's one of feature of getter-property),
        /// >
        /// > it will throw an exception at runtime with message `Literal value was NOT found` 
        /// > 
        /// > when executing `GetRawConstantValue` method of a `PropertyInfo` instance whose contains a getter-propert.
        /// </summary>
        public static void TestMethod7()
        {
            Console.WriteLine("In {0} nethod call," , MethodBase.GetCurrentMethod().Name);

            string propertyNameToFind = string.Empty;
            BindingFlags bindingFlags;

            Type type;
            PropertyInfo propertyInfo;

            string text = string.Empty;
            Planet jupiter = new Planet("Jupiter" , 3.65e08);
            Person personNico = new Person()
            {
                FirstName = "YazawaNico" ,
                LastName = "Nico" ,
            };
            int counter = 1;

            ///* ------ Example 1 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "planetName";
            bindingFlags = 
                BindingFlags.Instance | 
                BindingFlags.NonPublic
                ;

            type = jupiter.GetType();
            propertyInfo = type.GetProperty(propertyNameToFind, bindingFlags);

            try
            {
                if(propertyInfo != null)
                {
                    text = $"The {propertyNameToFind} property is found\nwhose raw value is {propertyInfo?.GetRawConstantValue() ?? "null"}";
                }
                else
                {
                    text = $"The {propertyNameToFind} property is NOT found\n";
                }

                Console.WriteLine(text);
                Console.WriteLine();
            }
            catch(Exception ex)
            {
                Console.BackgroundColor = ConsoleColor.DarkRed;

                Console.WriteLine(ex.Message);
            }
            finally
            {
                Console.BackgroundColor = ConsoleColor.Black;
            }

            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "FirstName";

            type = personNico.GetType();
            propertyInfo = type.GetProperty(propertyNameToFind);

            try
            {
                if(propertyInfo != null)
                {
                    text = $"The {propertyNameToFind} property is found\nwhose raw value is {propertyInfo?.GetRawConstantValue() ?? "null"}"; // throw an exception with message -- `Literal value was NOT found`
                }
                else
                {
                    text = $"The {propertyNameToFind} property is NOT found\n";
                }

                Console.WriteLine(text);
                Console.WriteLine();
            }
            catch(Exception ex)
            {
                Console.BackgroundColor = ConsoleColor.DarkRed;

                Console.WriteLine(ex.Message);
            }
            finally
            {
                Console.BackgroundColor = ConsoleColor.Black;
            }

            counter++;
        }
```

will output following

```
In TestMethod7 nethod call,
///* ------ Example 1 ------ *///
The planetName property is NOT found


///* ------ Example 2 ------ *///
Literal value was not found.
```

![Literal value was not found](Literal%20value%20was%20not%20found.png)

## reference
### API docs
+ [`PropertyInfo.GetRawConstantValue Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getrawconstantvalue?view=net-9.0)
