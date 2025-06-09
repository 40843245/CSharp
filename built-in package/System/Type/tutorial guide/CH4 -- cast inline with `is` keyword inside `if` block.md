# CH4 -- cast inline with `is` keyword inside `if` block
## objectives 
You will learn how to

+ cast inline with `is` keyword inside `if` block

## CH4.1 -- cast inline with `is` keyword inside `if` block
> [!CAUTION]
> It's only supported in `C#` version 7.0 (or above)

```
if(employeeNico is Person person1)
{
    // when `employeeNico` instance is `Person` type,
    // it will inline cast `employeeNico` instance to local scope variable named `person1`
    // thus, you can use `person1` variable here.
}
else
{
    // it will NOT inline cast `employeeNico` instance to local scope variable named `person1`
    // thus, you can NOT use `person1` variable here.
}
```
## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illstrate that cast inline in `is` expression inside `<conditionalStatement>` in `if(<conditionalStatement>)`
        /// 
        /// > [!CAUTION]
        /// > It's only supported in `C#` version 7.0 (or above)
        /// </summary>
        public static void TestMethod4()
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

            if(employeeNico is Person person1)
            {
                Console.WriteLine("The instance employeeNico is `Person` type. Info:{0}" , person1.ToString());
            }

            if(personNico is Employee employee1)
            {
                Console.WriteLine("The instance personNico is `Person` type. Info:{0}" , employee1.ToString());
            }
        }
```

will output following

```
In TestMethod4 method call
The instance employeeNico is `Person` type. Info:Example.DirtyBean.Employee
```

## reference
### Q&A
#### stackoverflow
+ [Andrew Hare's answer in `Type Checking: typeof, GetType, or is?`](https://stackoverflow.com/questions/983030/type-checking-typeof-gettype-or-is?rq=1)