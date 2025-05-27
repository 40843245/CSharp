# CH9 -- get specific runtime method info of specific type
## objectives
You will learn how to

+ get specific runtime method info of specific type

## CH9.1 -- get specific runtime method info of specific type
### `GetRuntimeMethod` instance method
To get specific runtime method info of specific type,

just simply invoking `GetRuntimeMethod` instance method.

`GetRuntimeMethod` instance method will return `MethodInfo`.

Syntax:

```
        //
        // 摘要:
        //     擷取表示指定之方法的物件。
        //
        // 參數:
        //   type:
        //     包含方法的型別。
        //
        //   name:
        //     方法的名稱。
        //
        //   parameters:
        //     陣列，其中包含方法的參數。
        //
        // 傳回:
        //     物件，表示指定的方法，如果找不到方法，則為 null。
        //
        // 例外狀況:
        //   T:System.ArgumentNullException:
        //     type 為 null。 -或- name 為 null。
        public static MethodInfo GetRuntimeMethod(this Type type , string name , Type [ ] parameters);
```
## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get specific runtime method info of specific class.
        /// </summary>
        public static void TestMethod14()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            EmployeeRepository employeeRepository = new EmployeeRepository();
            Type employeeRepositoryType = employeeRepository.GetType();
            var runtimeMethodInfo1 = employeeRepositoryType.GetRuntimeMethod("DeleteOne",new Type [ ] { });
            string text = string.Empty;

            if(runtimeMethodInfo1 != null)
            {
                text = $"`DeleteOne` method in class {employeeRepositoryType.FullName}:{runtimeMethodInfo1}\nName:{runtimeMethodInfo1.Name}";
            }
            else
            {
                text = $"`There is no `DeleteOne` method in class {employeeRepositoryType.FullName}";
            }
            Console.WriteLine(text);

            GenericRepository<Person> personRepository = new GenericRepository<Person>();
            Type personRepositoryType = personRepository.GetType();
            var runtimeMethodInfo2 = personRepositoryType.GetRuntimeMethod("DeleteOne" , new Type [ ] { });

            if(runtimeMethodInfo2 != null)
            {
                text = $"`DeleteOne` method in class {personRepositoryType.FullName}:{runtimeMethodInfo2}\nName:{runtimeMethodInfo2.Name}";
            }
            else
            {
                text = $"`There is no `DeleteOne` method in class {personRepositoryType.FullName}";
            }
            Console.WriteLine(text);

            Type highOrderFunctionsExtensionMethodsType = typeof(HighOrderFunctionsExtensionMethods);
            var runtimeMethodInfo3 = highOrderFunctionsExtensionMethodsType.GetRuntimeMethod("Apply" , new Type [ ] { });

            if(runtimeMethodInfo3 != null)
            {
                text = $"`Apply` method in class {highOrderFunctionsExtensionMethodsType.FullName}:{runtimeMethodInfo3}\nName:{runtimeMethodInfo3.Name}";
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
In TestMethod14 method call
`There is no `DeleteOne` method in class Example.Repositories.EmployeeRepository
`There is no `DeleteOne` method in class Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]
`There is no `Apply` method in class Example.Extensions.ExtensionMethods.HighOrderFunctionsExtensionMethods.HighOrderFunctionsExtensionMethods
```

## reference
### API docs
None