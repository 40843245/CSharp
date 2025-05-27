# CH15 -- get specific property info of specific class
## objectives
You will learn how to

+ get specific property info of specific class

## CH15.1 -- get specific property info of specific class
### `GetProperty` instance method
To get specific property info of specific class,

just simply invoking `GetProperty` instance method.

`GetProperty` instance method will return `PropertyInfo`.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get specific property info of specific class.
        /// </summary>
        public static void TestMethod17()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            string propertyToFind = "FirstName";
            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Type personNicoType = personNico.GetType();
            var propertyInfo = personNicoType.GetProperty(propertyToFind);
            string text = string.Empty;

            if(propertyInfo != null)
            {
                text = $"In `{personNicoType.Name}` type, there are {propertyInfo} property named `{propertyInfo.Name}`\n";            
            }
            else
            {
                text = $"In `{personNicoType.Name}` type, there are no property named `{propertyToFind}`";
            }
            Console.WriteLine(text);

            EmployeeRepository employeeRepository = new EmployeeRepository();

            Type employeeRepositoryType = employeeRepository.GetType();
            propertyInfo = employeeRepositoryType.GetProperty(propertyToFind);
            text = string.Empty;

            if(propertyInfo != null)
            {
                text = $"In `{employeeRepositoryType.Name}` type, there are {propertyInfo} property named `{propertyInfo.Name}`\n";
            }
            else
            {
                text = $"In `{employeeRepositoryType.Name}` type, there are no property named `{propertyToFind}`";
            }
            Console.WriteLine(text);
        }
```

will output following

```
In TestMethod17 method call
In `Person` type, there are System.String FirstName property named `FirstName`

In `EmployeeRepository` type, there are no property named `FirstName`
```

## reference
### API docs
+ [`Type.GetProperty Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getproperty?view=netframework-4.8.1)