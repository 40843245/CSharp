# CH1 -- properties of PropertyInfo
## objectives
You will know

+ properties of PropertyInfo

## CH1.1 -- properties of PropertyInfo
see [`PropertyInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo?view=net-9.0)

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// properties of `PropertyInfo`.
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} nethod call," , MethodBase.GetCurrentMethod().Name);

            string text = string.Empty;
            Type type;
            PropertyInfo [ ] propertyInfos;

            int counter = 1;

            ///* ------ Example 1 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            type = typeof(Company);
            propertyInfos = type.GetProperties();
            text = propertyInfos.GetInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            type = typeof(Person);
            propertyInfos = type.GetProperties();
            text = propertyInfos.GetInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod1 nethod call,
///* ------ Example 1 ------ *///
There are 2 properties:
Its name:Id
Is it a special name:False
Can this property be read:True
Can this property be written:True
Its property's type:Int32
Its get ancessor:get_Id
Its set ancessor:set_Id
Its set ancessor:set_Id

Its name:Name
Is it a special name:False
Can this property be read:True
Can this property be written:True
Its property's type:String
Its get ancessor:get_Name
Its set ancessor:set_Name
Its set ancessor:set_Name



///* ------ Example 2 ------ *///
There are 3 properties:
Its name:Id
Is it a special name:False
Can this property be read:True
Can this property be written:True
Its property's type:Int32
Its get ancessor:get_Id
Its set ancessor:set_Id
Its set ancessor:set_Id

Its name:FirstName
Is it a special name:False
Can this property be read:True
Can this property be written:True
Its property's type:String
Its get ancessor:get_FirstName
Its set ancessor:set_FirstName
Its set ancessor:set_FirstName

Its name:LastName
Is it a special name:False
Can this property be read:True
Can this property be written:True
Its property's type:String
Its get ancessor:get_LastName
Its set ancessor:set_LastName
Its set ancessor:set_LastName



```
## reference
### API docs
+ [`PropertyInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo?view=net-9.0)