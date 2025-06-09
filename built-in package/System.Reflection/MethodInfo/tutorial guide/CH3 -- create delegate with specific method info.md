# CH3 -- create delegate with specific method info
## objectives
You will learn how to

+ create delegate with specific method info

## CH3.1 -- CH3 -- create delegate with specific method info
### `CreateDelegate` instance method
Creates a delegate from this method.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to create delegate with current method (in `MethodInfo` type).
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Type type;
            string methodToFind;
            BindingFlags bindingFlags;
            MethodInfo methodInfo;

            string text = string.Empty;
            int counter = 1;

            ///* --------------- Example 1 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            type = typeof(Greeting);
            methodToFind = "Say";
            
            methodInfo = type.GetMethod(methodToFind);

            if(methodInfo != null)
            {
                text = string.Format("There are such method {0} found in {1} type" , methodToFind , type.FullName);
                Person personNico = new Person()
                {
                    Id = 1 ,
                    FirstName = "Yazawa" ,
                    LastName = "Nico" ,
                };

                Greeting greetingToNico =
                    new Greeting(
                        personNico,
                        "Welcome"
                    );

                Greeting.SayDelegate sayDelegate =
                    (Greeting.SayDelegate)methodInfo.CreateDelegate(
                        typeof(Greeting.SayDelegate) ,
                        greetingToNico
                    );

                sayDelegate();
            }
            else
            {
                text = string.Format("There are no such method {0} found in {1} type" , methodToFind , type.FullName);
            }

            Console.WriteLine(text);
            Console.WriteLine();
            counter++;

            ///* --------------- Example 2 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            type = typeof(Greeting);
            methodToFind = "StaticSay";

            methodInfo = type.GetMethod(methodToFind);

            if(methodInfo != null)
            {
                text = string.Format("There are such method {0} found in {1} type" , methodToFind , type.FullName);
                Person personNico = new Person()
                {
                    Id = 1 ,
                    FirstName = "Yazawa" ,
                    LastName = "Nico" ,
                };

                Greeting.StaticSayDelegate staticSayDelegate =
                    (Greeting.StaticSayDelegate)methodInfo.CreateDelegate(
                        typeof(Greeting.StaticSayDelegate) ,
                        null
                    );

                staticSayDelegate(personNico,"Welcome");
            }
            else
            {
                text = string.Format("There are no such method {0} found in {1} type" , methodToFind , type.FullName);
            }

            Console.WriteLine(text);
            Console.WriteLine();
            counter++;

        }
```

will output following

```
In TestMethod3 method call,
///* --------------- Example 1 --------------- *///
Welcome Yazawa Nico
There are such method Say found in Example.Helpers.Classes.Greeting type

///* --------------- Example 2 --------------- *///
Welcome Yazawa Nico
There are such method StaticSay found in Example.Helpers.Classes.Greeting type

```

## reference
### API docs
+ [`MethodInfo.CreateDelegate Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.createdelegate?view=net-8.0)