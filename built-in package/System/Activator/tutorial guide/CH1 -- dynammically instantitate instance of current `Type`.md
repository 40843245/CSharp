# CH1 -- dynammically instantitate instance of current `Type`
## objectives
You will learn how to

+ dynammically instantitate instance of current `Type`

## CH1.1 -- dynammically instantitate instance of current `Type`
### `Activator.CreateInstance` static method
Creates an instance of the specified type using the constructor that best matches the specified parameters.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to dynamically instaniate an instance which calls the constructor with no argument given current `Type` --
        /// 
        /// `Activator.CreateInstance` static method
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            object obj = Activator.CreateInstance(typeof(StringBuilder));

            StringBuilder stringBuilder = (StringBuilder)obj;
            stringBuilder.AppendLine("Hello World!");

            Console.WriteLine(stringBuilder.ToString());
        }
```

will output following

```
In TestMethod1 method call,
Hello World!

```

### example 2
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to dynamically instaniate an instance which calls the constructor with one or more arguments given current `Type` --
        /// 
        /// `Activator.CreateInstance` static method
        /// </summary>       
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            object [ ] parametersOfPerson = new object [ ]
            {
                1,
                "Yazawa",
                "Nico",
            };

            Person personNico = (Person)Activator.CreateInstance(typeof(Person), parametersOfPerson);

            object [ ] parametersOfGreeting = new object [ ]
            {
                personNico,
                "Welcome",
            };

            Greeting greetingToNico = (Greeting)Activator.CreateInstance(typeof(Greeting), parametersOfGreeting);

            greetingToNico.Say();
        }
```

will output following

```
In TestMethod2 method call,
Welcome Yazawa Nico
```

## reference
### API docs
+ [`Activator.CreateInstance Method`](https://learn.microsoft.com/en-us/dotnet/api/system.activator.createinstance?view=net-8.0)