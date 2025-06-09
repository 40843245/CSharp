# CH2 -- determines whether the custom attribute of the specified type or its derived types is applied to this parameter
## objectives
You will learn how to

+ determines whether the custom attribute of the specified type or its derived types is applied to this parameter

## CH2.1 -- determines whether the custom attribute of the specified type or its derived types is applied to this parameter
### `IsDefined` instance method
determines whether the custom attribute of the specified type or its derived types is applied to this parameter.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to check the parameter is defined by specific type or type from a parent class.
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // Get the type of the class 'MyClass1'.
            Type myType = typeof(MyClass1);
            // Get the members associated with the class 'MyClass1'.
            MethodInfo [ ] myMethods = myType.GetMethods();

            // For each method of the class 'MyClass1', display all the parameters
            // to which MyAttribute or its derived types have been applied.
            foreach(MethodInfo methodInfo in myMethods)
            {
                // Get the parameters for the method.
                ParameterInfo [ ] myParameters = methodInfo.GetParameters();
                if(myParameters.Length > 0)
                {
                    Console.WriteLine("\nThe following parameters of {0} have MyAttribute or a derived type: " , methodInfo);
                    foreach(ParameterInfo parameterInfo in myParameters)
                    {
                        if(parameterInfo.IsDefined(typeof(MyAttribute) , false))
                        {
                            Console.WriteLine("Parameter {0}, name = {1}, type = {2}" ,
                                parameterInfo.Position , parameterInfo.Name , parameterInfo.ParameterType);
                        }
                    }
                }
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod3 method call,

The following parameters of Void MyMethod(Int32, Int32, Int32) have MyAttribute or a derived type:
Parameter 0, name = i, type = System.Int32
Parameter 1, name = j, type = System.Int32

The following parameters of Boolean Equals(System.Object) have MyAttribute or a derived type:
End of TestMethod3 method call,
```

## reference
### API docs
+ [`ParameterInfo.IsDefined(Type, Boolean) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo.isdefined?view=net-9.0)

### examples
+ The code snippet in example 1 is referenced from the first example of [`ParameterInfo.IsDefined(Type, Boolean) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo.isdefined?view=net-9.0)