# CH4 -- set value of an property of current instance
## objectives
You will learn how to

+ set value of an property of current instance

## CH4.1 -- set value of an property of current instance
### `SetValue` instance method
Sets the property's value of current instance.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// set value of specific property of current instance.
        /// </summary>
        public static void TestMethod6()
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
                FirstName = "Yazawa" ,
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
            propertyInfo = type.GetProperty(propertyNameToFind , bindingFlags);


            if(propertyInfo != null)
            {
                text = $"The {propertyNameToFind} property is found\n";
                text += $"its original value is {propertyInfo.GetValue(jupiter)}\n";
                propertyInfo.SetValue(jupiter ,"mars");
                text += $"after change value with `SetValue` instance method, now its value is {propertyInfo.GetValue(jupiter)}\n";
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);
            Console.WriteLine();


            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "FirstName";

            type = personNico.GetType();
            propertyInfo = type.GetProperty(propertyNameToFind);

            if(propertyInfo != null)
            {
                text = $"The {propertyNameToFind} property is found\n";
                text += $"its original value is {propertyInfo.GetValue(personNico)}\n";
                propertyInfo.SetValue(personNico , "Ayase");
                text += $"after change value with `SetValue` instance method, now its value is {propertyInfo.GetValue(personNico)}\n";
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);
            Console.WriteLine();
            counter++;
        }
```

will output following

```
In TestMethod6 nethod call,
///* ------ Example 1 ------ *///
The planetName property is NOT found


///* ------ Example 2 ------ *///
The FirstName property is found
its original value is Yazawa
after change value with `SetValue` instance method, now its value is Ayase


```

## reference
### API docs
+ [`PropertyInfo.SetValue Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.setvalue?view=net-9.0)