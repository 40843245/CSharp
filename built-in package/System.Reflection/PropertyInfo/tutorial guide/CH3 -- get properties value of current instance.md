# CH3 -- get properties value of current instance
## objectives
You will learn how to

+ get properties value of current instance

## CH3.1 -- get properties value of current instance
### `GetValue` instance method
Returns `PropertyInfo[]` representing the properties value of a current instance.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// get properties value of current instance.
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} nethod call," , MethodBase.GetCurrentMethod().Name);

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

            text = Example.Extensions.
                    ExtensionMethods.PropertyInfoExtensionMethods.PropertyInfoExtensionMethods
                    .GetPropertyValuesInfo(jupiter);

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            text = Example.Extensions.
                    ExtensionMethods.PropertyInfoExtensionMethods.PropertyInfoExtensionMethods
                    .GetPropertyValuesInfo(personNico);

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod5 nethod call,
///* ------ Example 1 ------ *///
Type is: Planet
Properties (N = 2):
   Name (String): Jupiter
   Distance (Double): 365000000


///* ------ Example 2 ------ *///
Type is: Person
Properties (N = 3):
   Id (Int32): 0
   FirstName (String): YazawaNico
   LastName (String): Nico


```

## reference
### API docs
+ [`PropertyInfo.GetValue Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getvalue?view=net-9.0)

### examples
+ The code of example 1 is referenced from the first example in [`PropertyInfo.GetValue Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getvalue?view=net-9.0) and is modified.