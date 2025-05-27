# CH11 -- get specific method info of specific class
## objectives
You will learn how to

+ get specific method info of specific class

## CH11.1 -- get specific method info of specific class
### `GetMethod` instance method
To get specific method info of specific class,

just simply invoking `GetMethod` instance method.

`GetMethod` instance method will return `MethodInfo`.

## examples
### example 1
#### main code
Invoking following method

```
       /// <summary>
        /// illustrate how to get specific method info of specific class.
        /// </summary>
        public static void TestMethod15()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            EmployeeRepository employeeRepository = new EmployeeRepository();
            Type employeeRepositoryType = employeeRepository.GetType();
            var methodInfo1 = employeeRepositoryType.GetMethod("DeleteOne" , new Type [ ] { });
            string text = string.Empty;

            if(methodInfo1 != null)
            {
                text = $"`DeleteOne` method in class {employeeRepositoryType.FullName}:{methodInfo1}\nName:{methodInfo1.Name}";
            }
            else
            {
                text = $"`There is no `DeleteOne` method in class {employeeRepositoryType.FullName}";
            }
            Console.WriteLine(text);

            GenericRepository<Person> personRepository = new GenericRepository<Person>();
            Type personRepositoryType = personRepository.GetType();
            var methodInfo2 = personRepositoryType.GetMethod("DeleteOne" , new Type [ ] { });

            if(methodInfo2 != null)
            {
                text = $"`DeleteOne` method in class {personRepositoryType.FullName}:{methodInfo2}\nName:{methodInfo2.Name}";
            }
            else
            {
                text = $"`There is no `DeleteOne` method in class {personRepositoryType.FullName}";
            }
            Console.WriteLine(text);

            Type highOrderFunctionsExtensionMethodsType = typeof(HighOrderFunctionsExtensionMethods);
            var methodInfo3 = highOrderFunctionsExtensionMethodsType.GetMethod("Apply" , new Type [ ] { });

            if(methodInfo3 != null)
            {
                text = $"`Apply` method in class {highOrderFunctionsExtensionMethodsType.FullName}:{methodInfo3}\nName:{methodInfo3.Name}";
            }
            else
            {
                text = $"`There is no `Apply` method in class {highOrderFunctionsExtensionMethodsType.FullName}";
            }
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod15 method call
`There is no `DeleteOne` method in class Example.Repositories.EmployeeRepository
`There is no `DeleteOne` method in class Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
`There is no `Apply` method in class Example.Extensions.ExtensionMethods.HighOrderFunctionsExtensionMethods.HighOrderFunctionsExtensionMethods
```

## reference
### API docs
+ [`Type.GetMethod Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethod?view=netframework-4.8.1)