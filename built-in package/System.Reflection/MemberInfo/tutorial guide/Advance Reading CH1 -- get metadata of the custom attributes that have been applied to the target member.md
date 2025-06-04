# Advance Reading CH1 -- get metadata of the custom attributes that have been applied to the target member
## objectives
You will learn how to

+ get metadata of the custom attributes that have been applied to the target member

## Advance Reading CH1.1 -- get metadata of the custom attributes that have been applied to the target member
### `GetCustomAttributesData` instance method
returns `CustomAttributeData[]`. 

Each `CustomAttributeData` instance represents the metadata of the custom attribute that have been applied to the target member.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get metadata of the custom attributes that have been applied to the target member
        /// </summary>
        public static void TestMethod4()
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
In TestMethod4 method call,
   [Example.Classes.AttributeExample.MyAttribute("This is an example attribute.")]
      Constructor: 'Void .ctor(System.String)'
      Constructor arguments:
         Type: 'System.String'  Value: 'This is an example attribute.'
      Named arguments:
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
End of TestMethod4 method call,
```

## reference
### API docs
+ [`MemberInfo.GetCustomAttributesData Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo.getcustomattributesdata?view=net-9.0)

+ [`CustomAttributeData Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.customattributedata?view=net-9.0)

### exampples
+ In example 1, the definition of `OutputHandler.DisplayCustomAttributeData` static method

is referenced from the definition of `ShowAttributeData` in the first example of [`CustomAttributeData Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.customattributedata?view=net-9.0)