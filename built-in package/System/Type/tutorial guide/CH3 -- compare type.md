# CH3 -- compare type
## objectives
You will learn how to

+ compare type

## CH3.1 -- compare type
### `typeof` function
You can compare an variable or property is specific type of structure (`struct`) keyword using `typeof` function.

```
x.GetType() == typeof(int)
```

> [!CAUTION]
> You can NOT compare an variable or property is specific (your defined own) class using `typeof` function.
>
> Otherwise, you will get compile error.

> [!CAUTION]
> You can NOT also compare an variable or property is specific type of a variable or property using `typeof` function.
>
> Otherwise, you will get compile error.

### `is` keyword
You can also compare an variable or property is specific type using `is` keyword.

```
x is int
```

Additionally, you can also compare an variable or property is specific (your defined own) class.

```
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

x is Person
```

### `as` keyword and comparing null
Although one can not directly compare an variable or property is specific type of structure (`struct` keyword), specific type of a variable and a property, and specific (your defined own) class using `as` keyword, we can do that by enforcing type casting using `as` keyword (and which return the result after type casting), then compare the return value is equal to `null`.

> [!TIP]
> `as` keyword can safely enforce type casting, 
> 
> Take following example to illustrate how it works
> 
> ```
> public class Person
> {
>       public string FirstName { get; set; }
>       public string LastName { get; set; }
> }
>
> var variable1AfterCasting = x as Person
> ```
> 
> + If `x` can be cast info `Person` (here, `x` is Person type or its derived class), it will return the result after type casting.
>
> + Otherwise, it will return `null`.

> [!CAUTION]
> Under `C#` 8.0 version,
> 
> the type that trying to be cast using `as` keyword MUST be nullable type or referential type. 
>
> Otherwise, it will throw a compiler error.
>
> ![wrong usage of `as`.png](wrong%20usage%20of%20`as`.png)

Due to this feature, we can compare an variable or property is specific type of structure (`struct` keyword), specific type of a variable and a property, and specific (your defined own) class using `as` keyword by enforcing type casting using `as` keyword (and which return the result after type casting), then compare the return value is equal to `null`.

For example,

```
var variable1AfterCasting = x as int?
if(variable1AfterCasting != null)
{
    // x can be cast into `int?` (here, x can be converted to `int?`)
}
else
{
    // x can NOT be cast into `int?` (here, x can NOT be converted to `int?`)
}
```

```
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

var variable1AfterCasting = x as Person
if(variable1AfterCasting != null)
{
    // x can be cast into `Person` (here, x can be converted to `Person`, i.e. x is type of `Person` or its derived class)
}
else
{
    // x can NOT be cast into `Person` (here, x can NOT be converted to `Person`)
}
```

## examples
### example 1
#### code extension method
`IsTypeExtensionMethods.GetTypeComparisonOfClassesInfo` extension method is defined in `IsTypeExtensionMethods` static class as follows.

`IsTypeExtensionMethods.GetTypeComparisonOfClassesInfo` in `TypeExtensionMethods.cs`

```
    public static string GetTypeComparisonOfClassesInfo<TElement1, TElement2>      
        (
            this TElement1 element1,
            TElement2 element2

        )
            where TElement1 : class
            where TElement2 : class
        {
            string element1TypeFulleName = element1.GetType().FullName;
            string element2TypeFulleName = element2.GetType().FullName;
               
            StringBuilder stringBuilder = new StringBuilder();


            stringBuilder.AppendLine("Compare type using `is`");
            if(element1 is TElement2)
            {
                stringBuilder.AppendFormat("{0} is an {1} type\n", element1TypeFulleName,element2TypeFulleName);
            }
            else
            {
                stringBuilder.AppendFormat("{0} is NOT an {1} type\n" , element1TypeFulleName , element2TypeFulleName);
            }

            stringBuilder.AppendLine("Compare type using `as`.");
            if((element1 as TElement2) != null)
            {
                stringBuilder.AppendFormat("{0} is an {1} type\n" , element1TypeFulleName , element2TypeFulleName);
            }
            else
            {
                stringBuilder.AppendFormat("{0} is NOT an {1} type\n" , element1TypeFulleName , element2TypeFulleName);
            }

            return stringBuilder.ToString();
        }
```
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate comparing a variable or a property is specific type.
        /// `is` keyword v.s. `as` keyword
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Person personNico = new Person
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };

            Person personEli = new Person
            {
                FirstName = "Ayase" ,
                LastName = "Eli" ,
            };

            Employee employeeNico = new Employee
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
                Company = null
            };

            string infoText = string.Empty;

            infoText = personNico.GetTypeComparisonOfClassesInfo<Person , Person>(personEli);
            Console.WriteLine(infoText);

            infoText = personNico.GetTypeComparisonOfClassesInfo<Person , Employee>(employeeNico);
            Console.WriteLine(infoText);

            infoText = employeeNico.GetTypeComparisonOfClassesInfo<Employee , Person>(personNico);
            Console.WriteLine(infoText);
        }
```

will output following

```
In TestMethod3 method call
Compare type using `is`
Example.Bean.Person is an Example.Bean.Person type
Compare type using `as`.
Example.Bean.Person is an Example.Bean.Person type

Compare type using `is`
Example.Bean.Person is NOT an Example.DirtyBean.Employee type
Compare type using `as`.
Example.Bean.Person is NOT an Example.DirtyBean.Employee type

Compare type using `is`
Example.DirtyBean.Employee is an Example.Bean.Person type
Compare type using `as`.
Example.DirtyBean.Employee is an Example.Bean.Person type

```

## reference
### API docs
+ [`Type Class`](https://learn.microsoft.com/en-us/dotnet/api/system.type?view=net-8.0)

### docs
+ [`Type-testing operators and cast expressions - is, as, typeof, and casts`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/type-testing-and-cast)

### Q&A
#### stackoverflow
+ [`Type Checking: typeof, GetType, or is?`](https://stackoverflow.com/questions/983030/type-checking-typeof-gettype-or-is)

+ About the performance of `is` v.s. `typeof` keyword, see [willeM_ Van Onsem's answer in `Which is good to use: Object.GetType() == typeof(Type) or Object is Type? [duplicate]`](https://stackoverflow.com/questions/27813304/which-is-good-to-use-object-gettype-typeoftype-or-object-is-type/27813381#27813381)