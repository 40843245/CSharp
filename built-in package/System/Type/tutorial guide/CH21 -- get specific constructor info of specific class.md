# CH20 -- get specific constructor info of specific class
## objectives
You will learn how to

+ get specific constructor info of specific class

## CH20.1 -- get specific constructor info of specific class
### `GetConstructor` instance method
To get specific implemented interface info of specific class,

just simply invoke `GetConstructor` instance method.

#### Overloads
There are many overloads.

> [!CAUTION]
> Watch out the meaning in overloads. It is NOT intuitive.

see [`Type.GetConstructor Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getconstructor?view=net-9.0) for more details.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get one constructor info of specific class that matches the search.
        /// </summary>
        public static void TestMethod24()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            string text = string.Empty;
            
            BindingFlags bindingFlags;
            CallingConventions callingConventions;
            ParameterModifier[] parameterModifiers;
            Binder binder;


            ConstructorInfo constructorInfo;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type statitisticsHandlerType = typeof(StatisticsHandler);

            // get the default constructor of `StatisticsHandler` class. 
            constructorInfo = statitisticsHandlerType.GetConstructor(Type.EmptyTypes);
            
            if(constructorInfo != null)
            {
                text = $"The first found default constructor in {statitisticsHandlerType.FullName} type is {constructorInfo.GetType().FullName}.";
            }
            else
            {
                text = $"There are no default constructor found in {statitisticsHandlerType.FullName} type.";
            }

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Employee employeeNico = new Employee()
            {
                FirstName = "Yazawa",
                LastName = "Nico",
            };
            Type employeeNicoType = employeeNico.GetType();

            // get the default constructor of `StatisticsHandler` class. 
            constructorInfo = employeeNicoType.GetConstructor(Type.EmptyTypes);

            if(constructorInfo != null)
            {
                text = $"The first found default constructor in {employeeNicoType.FullName} type is {constructorInfo.GetType().FullName}.";
            }
            else
            {
                text = $"There are no default constructor found in {employeeNicoType.FullName} type.";
            }

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 3 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type demoClass1Type = typeof(DemoClass1);

            bindingFlags =
                BindingFlags.Instance |
                BindingFlags.Public
                ;

            callingConventions = CallingConventions.HasThis;

            binder = null;

            parameterModifiers = null;

            // get the default constructor of `StatisticsHandler` class. 
            constructorInfo = demoClass1Type.GetConstructor(
                bindingFlags,
                binder,
                callingConventions,
                new Type [ ] { demoClass1Type },
                parameterModifiers
            );

            if(constructorInfo != null)
            {
                text = $"The first found constructor in {demoClass1Type.FullName} type with search is {constructorInfo.GetType().FullName}.";
            }
            else
            {
                text = $"There are no default constructor found in {demoClass1Type.FullName} type with search.";
            }

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod24 method call
///* ------------------ Example 1 ------------------ *///
The first found default constructor in Example.Helpers.Numbers.StatisticsHandler type is System.Reflection.RuntimeConstructorInfo.

///* ------------------ Example 2 ------------------ *///
The first found default constructor in Example.DirtyBean.Employee type is System.Reflection.RuntimeConstructorInfo.

///* ------------------ Example 3 ------------------ *///
There are no default constructor found in Example.DemoClass.DemoClass1 type with search.

```

## reference
### API docs
+ [`Type.GetConstructor Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getconstructor?view=net-9.0)

+ [`ConstructorInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.constructorinfo?view=net-9.0)

+ [`Binder Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.binder?view=net-9.0)

+ [`BindingFlags Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.bindingflags?view=net-9.0)

+ [`CallingConventions Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.callingconventions?view=net-9.0)

+ [`ParameterModifier Struct`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parametermodifier?view=net-9.0)