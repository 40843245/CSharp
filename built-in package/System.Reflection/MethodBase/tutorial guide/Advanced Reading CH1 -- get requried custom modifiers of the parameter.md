# Advanced Reading CH1 -- get requried custom modifiers of the parameter
## objectives
You will learn how to

+ get requried custom modifiers

## Advanced Reading CH1.1 -- get requried custom modifiers of the parameter
### `GetRequiredCustomModifiers` instance method
get requried custom modifiers of the parameter

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get requried custom modifiers of the parameter.
        /// </summary>
        /// To truly demonstrate GetRequiredCustomModifiers, we'd need to
        /// compile some IL that includes a 'modreq'.
        /// For this C# example, we'll show how to reflect on an existing type,
        /// and provide a conceptual understanding of what GetRequiredCustomModifiers
        /// *would* find if such a modifier were present.
        /// Let's imagine a scenario where a parameter was compiled with a modreq.
        /// For instance, if you were dealing with code generated from F# or C++/CLI,
        /// you might encounter such modifiers.
        ///
        /// 
        /// A simplified example that shows how to access the method and its parameters
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Type myClassType = typeof(MyClass);
            MethodInfo myMethod = myClassType.GetMethod("MyMethod");

            if(myMethod != null)
            {
                ParameterInfo [ ] parameters = myMethod.GetParameters();

                foreach(ParameterInfo param in parameters)
                {
                    Console.WriteLine($"Parameter Name: {param.Name}");
                    Console.WriteLine($"Parameter Type: {param.ParameterType}");

                    Type [ ] requiredModifiers = param.GetRequiredCustomModifiers();

                    if(requiredModifiers.Length > 0)
                    {
                        Console.WriteLine("  Required Custom Modifiers:");
                        foreach(Type modifier in requiredModifiers)
                        {
                            Console.WriteLine($"    - {modifier.FullName}");
                        }
                    }
                    else
                    {
                        Console.WriteLine("  No Required Custom Modifiers.");
                    }

                    Type [ ] optionalModifiers = param.GetOptionalCustomModifiers();
                    if(optionalModifiers.Length > 0)
                    {
                        Console.WriteLine("  Optional Custom Modifiers:");
                        foreach(Type modifier in optionalModifiers)
                        {
                            Console.WriteLine($"    - {modifier.FullName}");
                        }
                    }
                    else
                    {
                        Console.WriteLine("  No Optional Custom Modifiers.");
                    }
                    Console.WriteLine();
                }
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod4 method call,
Parameter Name: someParameter
Parameter Type: System.Int32
  No Required Custom Modifiers.
  No Optional Custom Modifiers.

End of TestMethod4 method call,
```

## reference
### API docs
+ [`ParameterInfo.GetRequiredCustomModifiers Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo.getrequiredcustommodifiers?view=net-9.0)

### examples
+ The code snippet in example 1 is referenced from [Google Gemini Flash 2.5](https://g.co/gemini/share/98e86cbb8be7)