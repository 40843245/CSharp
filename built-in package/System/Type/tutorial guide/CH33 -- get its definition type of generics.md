# CH33 -- get its definition type of generics
## objectives
You will learn how to

+ get its definition type of generics

## CH33.1 -- get its definition type of generics
### `GenericDefinition` instance method
represents a generic type definition from which the current generic type can be constructed.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to represent a generic type definition from which the current generic type can be constructed.
        /// </summary>
        public static void TestMethod34()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type type;
            Type genericTypeDefinitionType;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GenericRepository<Person>);

            genericTypeDefinitionType = type.GetGenericTypeDefinition();

            Console.WriteLine("The {0} has GenericTypeDefinition {1}", type.FullName, genericTypeDefinitionType.FullName);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(IGenericRepository<Person>);

            genericTypeDefinitionType = type.GetGenericTypeDefinition();

            Console.WriteLine("The {0} has GenericTypeDefinition {1}" , type.FullName , genericTypeDefinitionType.FullName);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod34 method call
///* ------------------ Example 1 ------------------ *///
The Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] has GenericTypeDefinition Example.Generics.Repository.GenericRepository`1

///* ------------------ Example 2 ------------------ *///
The Example.Generics.Repository.IGenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] has GenericTypeDefinition Example.Generics.Repository.IGenericRepository`1

```

## reference
### API docs
+ [`Type.GetGenericTypeDefinition Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getgenerictypedefinition?view=net-8.0)