# CH19 -- get specific implemented interface info of specific class
## objectives
You will learn how to

+ get specific implemented interface info of specific class

## CH19.1 -- get specific implemented interface info of specific class
### `GetInterface` instance method
To get specific implemented interface info of specific class,

just simply invoke `GetInterface` instance method.

> [!CAUTION]
> Watch out the name to search for.
>
> If one want to search specific implemented *generic* interface,
> 
> The name will be mangled.
> 
> For example,
> 
> One want to search "IGenericRepository<string>", the mangled name will be `IGenericRepository`1"
> 
> Think of this:
> 
> ```
> var type = Type(new IGenericRepository());
> Console.WriteLine(type.Name);
> ```
>
> It might output:
>
> IGenericRepository`1
>
> ![remark of searching generic interfaces](remark%20of%20searching%20generic%20interfaces.png)
> 
> For more details, see [`Type.GetInterface Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getinterface?view=netframework-4.8.1)


## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get specific implemented interface info of specific class.
        /// </summary>
        public static void TestMethod19()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            /// Reflecting "IGenericRepository" type will get name "IGenericRepository`1" 
            /// 
            /// Think of this:
            /// 
            /// ```
            /// var type = Type(new IGenericRepository());
            /// Console.WriteLine(type.Name);
            /// ```
            /// 
            /// It might output:
            /// 
            /// IGenericRepository`1
            /// 
            /// For more details, see [`Type.GetInterface Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getinterface?view=netframework-4.8.1)
            string interfaceToFind = "IGenericRepository`1"; 
            
            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var interfaceInfo = personNicoType.GetInterface(interfaceToFind);
            string text = string.Empty;

            if(interfaceInfo != null)
            {
                text = $"In `{personNicoType.Name}` type, it implements {interfaceInfo.Name} interface\n";
            }
            else
            {
                text = $"In `{personNicoType.Name}` type, it does NOT implement {interfaceToFind} interface\n";
            }
            Console.WriteLine(text);

            EmployeeRepository employeeRepository = new EmployeeRepository();

            Type employeeRepositoryType = employeeRepository.GetType();
            interfaceInfo = employeeRepositoryType.GetInterface(interfaceToFind);
            text = string.Empty;

            if(interfaceInfo != null)
            {
                text = $"In `{employeeRepositoryType.Name}` type, it implements {interfaceInfo.Name} interface\n";
            }
            else
            {
                text = $"In `{employeeRepositoryType.Name}` type, it does NOT implement {interfaceToFind} interface\n";
            }
            Console.WriteLine(text);

            GenericRepository<Person> personRepository = new GenericRepository<Person>();

            Type personRepositoryType = personRepository.GetType();
            interfaceInfo = personRepositoryType.GetInterface(interfaceToFind);
            text = string.Empty;

            if(interfaceInfo != null)
            {
                text = $"In `{personRepositoryType.Name}` type, it implements {interfaceInfo.Name} interface\n";
            }
            else
            {
                text = $"In `{personRepositoryType.Name}` type, it does NOT implement {interfaceToFind} interface\n";
            }
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod19 method call
In `Person` type, it does NOT implement IGenericRepository`1 interface

In `EmployeeRepository` type, it implements IGenericRepository`1 interface

In `GenericRepository`1` type, it implements IGenericRepository`1 interface

```

## reference
### API docs
+ [`Type.GetInterface Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getinterface?view=netframework-4.8.1)