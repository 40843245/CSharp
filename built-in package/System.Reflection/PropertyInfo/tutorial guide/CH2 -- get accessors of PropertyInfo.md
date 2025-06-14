# CH2 -- get accessors of PropertyInfo
## objectives
You will learn how to 

+ get `get` and `set` accessors of `PropertyInfo`

+ get `get` accessors of `PropertyInfo`

+ get `set` accessors of `PropertyInfo`

## CH2.1 -- get `get` and `set` accessors of `PropertyInfo`
### `GetAccessors` instance method
Returns an array of `MethodInfo[]` whose elements reflect the public `get` and `set` accessors of the property reflected by the current instance.

## CH2.2 -- get `set` accessors of `PropertyInfo`
### `GetAccessors` instance method
Returns `MethodInfo?` that reflects the public `get` accessors of the property reflected by the current instance.

## CH2.3 -- get `set` accessors of `PropertyInfo`
### `GetAccessors` instance method
Returns `MethodInfo?` that reflects the public `set` accessors of the property reflected by the current instance.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// get accessors of `PropertyInfo`.
        /// </summary>
        public static void TestMethod2()
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
            text = propertyInfos.GetAccessorsInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            type = typeof(Person);
            propertyInfos = type.GetProperties();
            text = propertyInfos.GetAccessorsInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following 

```
In TestMethod2 nethod call,
///* ------ Example 1 ------ *///
There are 2 properties:
 + There are 2 methods:
  + Name:get_Id
  + DeclaringType:Example.Bean.Company

  + Name:set_Id
  + DeclaringType:Example.Bean.Company


 + There are 2 methods:
  + Name:get_Name
  + DeclaringType:Example.Bean.Company

  + Name:set_Name
  + DeclaringType:Example.Bean.Company




///* ------ Example 2 ------ *///
There are 3 properties:
 + There are 2 methods:
  + Name:get_Id
  + DeclaringType:Example.Bean.Person

  + Name:set_Id
  + DeclaringType:Example.Bean.Person


 + There are 2 methods:
  + Name:get_FirstName
  + DeclaringType:Example.Bean.Person

  + Name:set_FirstName
  + DeclaringType:Example.Bean.Person


 + There are 2 methods:
  + Name:get_LastName
  + DeclaringType:Example.Bean.Person

  + Name:set_LastName
  + DeclaringType:Example.Bean.Person



```

### example 2
#### main code
Invoking following method

```
        /// <summary>
        /// get `get` accessors of `PropertyInfo`.
        /// </summary>
        public static void TestMethod3()
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
            text = propertyInfos.GetGetMethodInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            type = typeof(Person);
            propertyInfos = type.GetProperties();
            text = propertyInfos.GetGetMethodInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod3 nethod call,
///* ------ Example 1 ------ *///
There are 2 properties:
 + Name:get_Id
 + DeclaringType:Example.Bean.Company

 + Name:get_Name
 + DeclaringType:Example.Bean.Company



///* ------ Example 2 ------ *///
There are 3 properties:
 + Name:get_Id
 + DeclaringType:Example.Bean.Person

 + Name:get_FirstName
 + DeclaringType:Example.Bean.Person

 + Name:get_LastName
 + DeclaringType:Example.Bean.Person



```

### example 3
#### main code
Invoking following method

```
        /// <summary>
        /// get `set` accessors of `PropertyInfo`.
        /// </summary>
        public static void TestMethod4()
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
            text = propertyInfos.GetSetMethodInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            type = typeof(Person);
            propertyInfos = type.GetProperties();
            text = propertyInfos.GetSetMethodInfo();
            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod4 nethod call,
///* ------ Example 1 ------ *///
There are 2 properties:
 + Name:set_Id
 + DeclaringType:Example.Bean.Company

 + Name:set_Name
 + DeclaringType:Example.Bean.Company



///* ------ Example 2 ------ *///
There are 3 properties:
 + Name:set_Id
 + DeclaringType:Example.Bean.Person

 + Name:set_FirstName
 + DeclaringType:Example.Bean.Person

 + Name:set_LastName
 + DeclaringType:Example.Bean.Person



```

## reference
### API docs
+ [`PropertyInfo.GetAccessors Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getaccessors?view=net-9.0)

+ [`PropertyInfo.GetGetMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getgetmethod?view=net-9.0)

+ [`PropertyInfo.GetSetMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getsetmethod?view=net-9.0)