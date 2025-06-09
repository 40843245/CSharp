# CH1 -- implement IConvertible
## objectives
You will learn how to

+ implement IConvertible

## CH1.1 -- implement IConvertible
When one defines a class that implements `IConvertible` interface under `System` namespace,

one have to define all methods in `IConvertible`, 

including these methods

+ `GetTypeCode`
+ `ToBoolean`
+ `ToByte`
+ `ToChar`
+ `ToDateTime`
+ `ToDecimal`
+ `ToDouble`
+ `ToInt16`
+ `ToInt32`
+ `ToInt64`
+ `ToSByte`
+ `ToSingle`
+ `ToString`
+ `ToType`
+ `ToUInt16`
+ `ToUInt32`
+ `ToUInt64`

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to implement `IConvertible` interface.
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            CultureInfo cultureInfo = CultureInfo.CurrentCulture;
            NumberFormatInfo numberFormatInfo = cultureInfo.NumberFormat;
            DateTimeFormatInfo dateTimeFormatInfo = cultureInfo.DateTimeFormat;
      
            Person personNico = new Person()
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
                Id = 1 ,
            };

            Greeting greetingToNico = 
                new Greeting(
                    person: personNico,
                    greetingMessage:"Welcome"
                );

            Console.WriteLine(greetingToNico.ToString());

            Console.WriteLine("nico's id:{0}",greetingToNico.ToDecimal(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToInt16(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToInt32(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToInt64(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToUInt16(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToUInt32(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToUInt64(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToByte(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToSByte(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToSingle(numberFormatInfo));
            Console.WriteLine("nico's id:{0}",greetingToNico.ToDouble(numberFormatInfo));
            Console.WriteLine("{0}'s first char is {1}",greetingToNico.ToString(),greetingToNico.ToChar(numberFormatInfo));
            Console.WriteLine("nico's typeCode:{0}",greetingToNico.GetTypeCode().ToString());

            Console.WriteLine();

            if(greetingToNico is IConvertible iConvertibleGreetingToNico)
            {
                Console.WriteLine("{0} is IConvertible" , greetingToNico.ToString());
             
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToDecimal(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToInt16(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToInt32(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToInt64(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToUInt16(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToUInt32(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToUInt64(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToByte(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToSByte(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToSingle(numberFormatInfo));
                Console.WriteLine("nico's id:{0}" , iConvertibleGreetingToNico.ToDouble(numberFormatInfo));
                Console.WriteLine("{0}'s first char is {1}" , iConvertibleGreetingToNico.ToString() , iConvertibleGreetingToNico.ToChar(numberFormatInfo));
                Console.WriteLine("nico's typeCode:{0}" , iConvertibleGreetingToNico.GetTypeCode().ToString());
            }
            else
            {
                Console.WriteLine("{0} is NOT IConvertible" , greetingToNico.ToString());
            }
        }
```

will output following

```
In TestMethod1 method call
Example.Helpers.Classes.Greeting
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
Example.Helpers.Classes.Greeting's first char is Y
nico's typeCode:String

Example.Helpers.Classes.Greeting is IConvertible
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
nico's id:1
Example.Helpers.Classes.Greeting's first char is Y
nico's typeCode:String
```

## reference
### API docs
+ [`IConvertible Interface`](https://learn.microsoft.com/en-us/dotnet/api/system.iconvertible?view=net-9.0)