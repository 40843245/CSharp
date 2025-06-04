# Advance Reading CH1 -- get metadata of the custom attributes that have been applied to the target parameter
## objectives
You will learn how to

+ get metadata of the custom attributes that have been applied to the target parameter

## Advance Reading CH1.1 -- get metadata of the custom attributes that have been applied to the target parameter
### `GetCustomAttributesData` instance method (inherited from `MemberInfo`)
returns `CustomAttributeData[]`. 

Each `CustomAttributeData` instance represents the metadata of the custom attribute that have been applied to the target parameter.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get metadata of the custom attributes that have been applied to the target parameter
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            try
            {
                // Get the type of MyClass1.
                Type myClass1Type = typeof(MyClass1);


                // Get the members associated with MyClass1.
                MemberInfo [ ] myClass1Members = myClass1Type.GetMembers();

                // Display the attributes for each of the members of MyClass1.
                for(int i = 0; i < myClass1Members.Length; i++)
                {
                    MemberInfo myClass1Member = myClass1Members [ i ];
                    IList<CustomAttributeData> myClass1CustomAttributeDatas = myClass1Member.GetCustomAttributesData();

                    OutputHandler.DisplayCustomAttributeData(myClass1CustomAttributeDatas);
                }
            }
            catch(Exception e)
            {
                Console.WriteLine("An exception occurred: {0}" , e.Message);
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod5 method call,
   [__DynamicallyInvokableAttribute()]
      Constructor: 'Void .ctor()'
      Constructor arguments:
      Named arguments:
   [__DynamicallyInvokableAttribute()]
      Constructor: 'Void .ctor()'
      Constructor arguments:
      Named arguments:
   [System.Security.SecuritySafeCriticalAttribute()]
      Constructor: 'Void .ctor()'
      Constructor arguments:
      Named arguments:
   [__DynamicallyInvokableAttribute()]
      Constructor: 'Void .ctor()'
      Constructor arguments:
      Named arguments:
   [__DynamicallyInvokableAttribute()]
      Constructor: 'Void .ctor()'
      Constructor arguments:
      Named arguments:
End of TestMethod5 method call,
```

## reference
### API docs
+ [`ParameterInfo.GetCustomAttributesData`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo.getcustomattributesdata?view=net-9.0)

+ [`CustomAttributeData Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.customattributedata?view=net-9.0)

### exampples
+ In example 1, the definition of `OutputHandler.DisplayCustomAttributeData` static method

is referenced from the definition of `ShowAttributeData` in the first example of [`CustomAttributeData Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.customattributedata?view=net-9.0)