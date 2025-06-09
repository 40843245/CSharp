# CH17 -- get specific field info of specific class
## objectives
You will learn how to

+ get specific field info of specific class

## CH17.1 -- get specific field info of specific class
### `GetField` instance method
To get specific field info of specific class,

just simply invoking `GetField` instance method.

`GetField` instance method will return `FieldInfo`.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get specific field info of specific class.
        /// </summary>
        public static void TestMethod18()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            string fieldToFind = "FirstName";
            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var fieldInfo = personNicoType.GetField(fieldToFind);
            string text = string.Empty;

            if(fieldInfo != null)
            {
                text = $"In `{personNicoType.Name}` type, there are {fieldInfo} field named `{fieldInfo.Name}`\n";
            }
            else
            {
                text = $"In `{personNicoType.Name}` type, there are no field named `{fieldToFind}`";
            }
            Console.WriteLine(text);

            EmployeeRepository employeeRepository = new EmployeeRepository();

            Type employeeRepositoryType = employeeRepository.GetType();
            fieldInfo = employeeRepositoryType.GetField(fieldToFind);
            text = string.Empty;

            if(fieldInfo != null)
            {
                text = $"In `{employeeRepositoryType.Name}` type, there are {fieldInfo} field named `{fieldInfo.Name}`\n";
            }
            else
            {
                text = $"In `{employeeRepositoryType.Name}` type, there are no field named `{fieldToFind}`";
            }
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod18 method call
In `Person` type, there are no field named `FirstName`
In `EmployeeRepository` type, there are no field named `FirstName`
```

## reference
### API docs
+ [`Type.GetField Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getfield?view=netframework-4.8.1)