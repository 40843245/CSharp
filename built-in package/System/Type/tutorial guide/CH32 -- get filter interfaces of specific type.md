# CH32 -- get filter interfaces of specific type
## objectives
You will learn how to

+ get filter interfaces of specific type

## CH32.1 get filter interfaces of specific type
### `FindInterfaces` instance method
find interfaces implemented or inherited by the current Type by filter.

## examples
### example 1
#### main code
Invoking following method

```
       /// <summary>
        /// find interfaces by filters from interfaces implemented or inherited by the current Type.
        /// </summary>
        public static void TestMethod37()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            try
            {
                XmlDocument myXMLDoc = new XmlDocument();
                myXMLDoc.LoadXml("<book genre='novel' ISBN='1-861001-57-5'>" +
                    "<title>Pride And Prejudice</title>" + "</book>");
                Type myType = myXMLDoc.GetType();

                // Specify the TypeFilter delegate that compares the
                // interfaces against filter criteria.
                TypeFilter myFilter = new TypeFilter(MyInterfaceFilter.GetInterfaceFilter);
                String [ ] myInterfaceList = new String [ 2 ]
                    {"System.Collections.IEnumerable",
                "System.Collections.ICollection"};
                for(int index = 0; index < myInterfaceList.Length; index++)
                {
                    Type [ ] myInterfaces = myType.FindInterfaces(myFilter ,
                        myInterfaceList [ index ]);
                    if(myInterfaces.Length > 0)
                    {
                        Console.WriteLine("\n{0} implements the interface {1}." ,
                            myType , myInterfaceList [ index ]);
                        for(int j = 0; j < myInterfaces.Length; j++)
                            Console.WriteLine("Interfaces supported: {0}." ,
                                myInterfaces [ j ].ToString());
                    }
                    else
                        Console.WriteLine(
                            "\n{0} does not implement the interface {1}." ,
                            myType , myInterfaceList [ index ]);
                }
            }
            catch(ArgumentNullException e)
            {
                Console.WriteLine("ArgumentNullException: " + e.Message);
            }
            catch(TargetInvocationException e)
            {
                Console.WriteLine("TargetInvocationException: " + e.Message);
            }
            catch(Exception e)
            {
                Console.WriteLine("Exception: " + e.Message);
            }

            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod37 method call
///* ------------------ Example 1 ------------------ *///

System.Xml.XmlDocument implements the interface System.Collections.IEnumerable.
Interfaces supported: System.Collections.IEnumerable.

System.Xml.XmlDocument does not implement the interface System.Collections.ICollection.

```

## reference
### API docs
+ [`Type.FindInterfaces(TypeFilter, Object) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.findinterfaces?view=net-8.0)

### examples
+ The code of example is modified from example in [`Type.FindInterfaces(TypeFilter, Object) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.findinterfaces?view=net-8.0)