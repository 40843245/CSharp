# CH2 -- determines whether the custom attribute of the specified type or its derived types is applied to this member
## objectives
You will learn how to

+ determines whether the custom attribute of the specified type or its derived types is applied to this member

## CH2.1 -- determines whether the custom attribute of the specified type or its derived types is applied to this member
### `IsDefined` instance method
determines whether the custom attribute of the specified type or its derived types is applied to this member.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to check the member is defined by specific type or type from a parent class.
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            try
            {
                // Get the type of MyClass1.
                Type myType = typeof(MyClass1);
                // Get the members associated with MyClass1.
                MemberInfo [ ] myMembers = myType.GetMembers();

                // Display the attributes for each of the members of MyClass1.
                for(int i = 0; i < myMembers.Length; i++)
                {
                    // Display the attribute if it is of type MyAttribute.
                    if(myMembers [ i ].IsDefined(typeof(MyAttribute) , false))
                    {
                        Object [ ] myAttributes = myMembers [ i ].GetCustomAttributes(typeof(MyAttribute) , false);
                        Console.WriteLine("\nThe attributes of type MyAttribute for the member {0} are: \n" ,
                            myMembers [ i ]);
                        for(int j = 0; j < myAttributes.Length; j++)
                            // Display the value associated with the attribute.
                            Console.WriteLine("The value of the attribute is : \"{0}\"" ,
                                ((MyAttribute)myAttributes [ j ]).Name);
                    }
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
In TestMethod3 method call,

The attributes of type MyAttribute for the member Void MyMethod(Int32) are:

The value of the attribute is : "This is an example attribute."
End of TestMethod3 method call,
```

## reference
### API docs
+ [`MemberInfo.IsDefined(Type, Boolean) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo.isdefined?view=net-9.0)

### examples
+ The code snippet in example 1 is referenced from the first example of [`MemberInfo.IsDefined(Type, Boolean) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo.isdefined?view=net-9.0)