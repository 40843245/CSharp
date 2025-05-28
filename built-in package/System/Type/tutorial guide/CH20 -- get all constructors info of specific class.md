# CH20 -- get all constructors info of specific class
## objectives
You will learn how to

+ get all constructors info of specific class

## CH20.1 -- get all constructors info of specific class
### `GetConstructors` instance method
To get specific implemented interface info of specific class,

just simply invoke `GetConstructors` instance method.

#### Overloads
There are two overloads.

> [!CAUTION]
> Watch out the meaning in overloads. It is NOT intuitive.

+ `GetConstructors()`:
returns all the **public** constructors defined for the current Type.

+ `GetConstructors(BindingFlags bindingflags)`:
returns the constructors that match the search determined by `bindingflags`.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get constructors info of specific class.
        /// </summary>
        public static void TestMethod23()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;

            BindingFlags bindingFlags;

            ConstructorInfo [ ] constructorInfos;
            
            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            GenericRepository<Person> personRepository = new GenericRepository<Person>();
            Type personRepositoryType = personRepository.GetType();

            constructorInfos = personRepositoryType.GetConstructors();

            Console.WriteLine("The {0} type has {1} public, non-static constructor(s)", personRepositoryType.FullName,constructorInfos.Length);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Employee employeeNico = new Employee()
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type employeeNicoType = employeeNico.GetType();

            constructorInfos = employeeNicoType.GetConstructors();

            Console.WriteLine("The {0} type has {1} public, non-static constructor(s)" , employeeNicoType.FullName , constructorInfos.Length);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 3------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type iPersonGenericRepositoryType = typeof(IGenericRepository<Person>);

            constructorInfos = iPersonGenericRepositoryType.GetConstructors();

            Console.WriteLine("The {0} type has {1} public, non-static constructor(s)" , iPersonGenericRepositoryType.FullName , constructorInfos.Length);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 4------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type statisticsHandlerType = typeof(StatisticsHandler);

            bindingFlags =
                BindingFlags.NonPublic
                ;

            constructorInfos = statisticsHandlerType.GetConstructors(bindingFlags);
            
            Console.WriteLine("The {0} type has {1} non-public constructor(s)" , statisticsHandlerType.FullName , constructorInfos.Length);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 5------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);
       
            bindingFlags =
                BindingFlags.Default
                ;

            constructorInfos = statisticsHandlerType.GetConstructors(bindingFlags);

            Console.WriteLine("The {0} type has {1} constructor(s) due to bindingFlags is set to `BindingFlags.Default`." , statisticsHandlerType.FullName , constructorInfos.Length);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 6------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type objectType = typeof(object);

            constructorInfos = objectType.GetConstructors();

            Console.WriteLine("The {0} type has {1} constructor(s)." , objectType.FullName , constructorInfos.Length);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod23 method call
///* ------------------ Example 1 ------------------ *///
The Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type has 1 public, non-static constructor(s)

///* ------------------ Example 2 ------------------ *///
The Example.DirtyBean.Employee type has 1 public, non-static constructor(s)

///* ------------------ Example 3 ------------------ *///
The Example.Generics.Repository.IGenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type has 0 public, non-static constructor(s)

///* ------------------ Example 4 ------------------ *///
The Example.Helpers.Numbers.StatisticsHandler type has 0 non-public constructor(s)

///* ------------------ Example 5 ------------------ *///
The Example.Helpers.Numbers.StatisticsHandler type has 0 constructor(s) due to bindingFlags is set to `BindingFlags.Default`.

///* ------------------ Example 6 ------------------ *///
The System.Object type has 1 constructor(s).

```

## reference
### API docs
+ [`Type.GetConstructors Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getconstructors?view=net-9.0)