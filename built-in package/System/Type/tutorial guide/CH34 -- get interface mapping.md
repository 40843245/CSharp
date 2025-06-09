# CH34 -- get interface mapping
## objectives
You will learn how to

+ get interface mapping

## CH34.1 -- get interface mapping
### `GetInterfaceMap` instance method
returns an interface mapping for the specified interface type.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get interface mapping.
        /// </summary>
        public static void TestMethod35()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type iShapeType = typeof(IShape);

            // --- Example 1: Finding an implementation in Circle (explicit implementation) ---
            Type circleType = typeof(Circle);
            Console.WriteLine($"\n--- Analyzing '{circleType.Name}' for '{iShapeType.Name}' members ---");

            // Get the PropertyInfo for IShape.Area
            PropertyInfo iShapeAreaProperty = iShapeType.GetProperty("Area");
            if(iShapeAreaProperty != null)
            {
                Console.WriteLine($"\nInterface Property: {iShapeAreaProperty.Name}");
                // To find the implementing property, we use GetInterfaceMap
                // GetInterfaceMap provides the mapping between interface members and class members.
                InterfaceMapping map = circleType.GetInterfaceMap(iShapeType);

                for(int i = 0; i < map.InterfaceMethods.Length; i++)
                {
                    // We're interested in the getter method of the property
                    if(map.InterfaceMethods [ i ] == iShapeAreaProperty.GetMethod)
                    {
                        MethodInfo implementingMethod = map.TargetMethods [ i ];
                        // The implementingMethod is the getter of the property
                        // To get the PropertyInfo from the MethodInfo:
                        PropertyInfo implementingProperty = implementingMethod.DeclaringType
                                                              .GetProperties(BindingFlags.Public | BindingFlags.NonPublic | BindingFlags.Instance)
                                                              .FirstOrDefault(p => (p.GetMethod == implementingMethod || p.SetMethod == implementingMethod));

                        if(implementingProperty != null)
                        {
                            Console.WriteLine($"  Corresponding Implementing Property: {implementingProperty.DeclaringType.Name}.{implementingProperty.Name} (IsExplicit: {implementingMethod.IsPrivate})");
                        }
                        else
                        {
                            Console.WriteLine("  Could not resolve implementing property from method.");
                        }
                        break;
                    }
                }
            }

            // Get the MethodInfo for IShape.Draw
            MethodInfo iShapeDrawMethod = iShapeType.GetMethod("Draw");
            if(iShapeDrawMethod != null)
            {
                Console.WriteLine($"\nInterface Method: {iShapeDrawMethod.Name}");
                InterfaceMapping map = circleType.GetInterfaceMap(iShapeType);

                for(int i = 0; i < map.InterfaceMethods.Length; i++)
                {
                    if(map.InterfaceMethods [ i ] == iShapeDrawMethod)
                    {
                        MethodInfo implementingMethod = map.TargetMethods [ i ];
                        Console.WriteLine($"  Corresponding Implementing Method: {implementingMethod.DeclaringType.Name}.{implementingMethod.Name} (IsExplicit: {implementingMethod.IsPrivate})");
                        break;
                    }
                }
            }

            // Get the EventInfo for IShape.OnCalculated
            EventInfo iShapeOnCalculatedEvent = iShapeType.GetEvent("OnCalculated");
            if(iShapeOnCalculatedEvent != null)
            {
                Console.WriteLine($"\nInterface Event: {iShapeOnCalculatedEvent.Name}");
                InterfaceMapping map = circleType.GetInterfaceMap(iShapeType);

                // Events are tricky. We need to find the add/remove methods.
                // Let's focus on the 'Add' method for simplicity.
                MethodInfo iShapeAddMethod = iShapeOnCalculatedEvent.GetAddMethod();

                for(int i = 0; i < map.InterfaceMethods.Length; i++)
                {
                    if(map.InterfaceMethods [ i ] == iShapeAddMethod)
                    {
                        MethodInfo implementingAddMethod = map.TargetMethods [ i ];
                        // Now, find the EventInfo that uses this add method
                        EventInfo implementingEvent = implementingAddMethod.DeclaringType
                            .GetEvents(BindingFlags.Public | BindingFlags.NonPublic | BindingFlags.Instance)
                            .FirstOrDefault(e => e.GetAddMethod(true) == implementingAddMethod || e.GetRemoveMethod(true) == implementingAddMethod);

                        if(implementingEvent != null)
                        {
                            Console.WriteLine($"  Corresponding Implementing Event: {implementingEvent.DeclaringType.Name}.{implementingEvent.Name} (IsExplicit: {implementingAddMethod.IsPrivate})");
                        }
                        else
                        {
                            Console.WriteLine("  Could not resolve implementing event from method.");
                        }
                        break;
                    }
                }
            }

            counter++;

            // --- Example 2: Finding an implementation in Square (implicit implementation) ---
            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type squareType = typeof(Square);
            Console.WriteLine($"\n--- Analyzing '{squareType.Name}' for '{iShapeType.Name}' members ---");

            // For implicitly implemented members, you can often just use the member's name
            // on the concrete type, but GetInterfaceMap is the robust way for both.

            // Get the PropertyInfo for IShape.Area
            iShapeAreaProperty = iShapeType.GetProperty("Area");
            if(iShapeAreaProperty != null)
            {
                Console.WriteLine($"\nInterface Property: {iShapeAreaProperty.Name}");
                InterfaceMapping map = squareType.GetInterfaceMap(iShapeType);

                for(int i = 0; i < map.InterfaceMethods.Length; i++)
                {
                    if(map.InterfaceMethods [ i ] == iShapeAreaProperty.GetMethod)
                    {
                        MethodInfo implementingMethod = map.TargetMethods [ i ];
                        PropertyInfo implementingProperty = implementingMethod.DeclaringType
                                                              .GetProperties(BindingFlags.Public | BindingFlags.NonPublic | BindingFlags.Instance)
                                                              .FirstOrDefault(p => (p.GetMethod == implementingMethod || p.SetMethod == implementingMethod));

                        if(implementingProperty != null)
                        {
                            Console.WriteLine($"  Corresponding Implementing Property: {implementingProperty.DeclaringType.Name}.{implementingProperty.Name} (IsExplicit: {implementingMethod.IsPrivate})");
                        }
                        else
                        {
                            Console.WriteLine("  Could not resolve implementing property from method.");
                        }
                        break;
                    }
                }
            }

            counter++;
        }
```

will output following

```
In TestMethod35 method call
///* ------------------ Example 1 ------------------ *///

--- Analyzing 'Circle' for 'IShape' members ---

Interface Property: Area
  Corresponding Implementing Property: Circle.Example.Helpers.Graphics.IShape.Area (IsExplicit: True)

Interface Method: Draw
  Corresponding Implementing Method: Circle.Draw (IsExplicit: False)

Interface Event: OnCalculated
  Corresponding Implementing Event: Circle.OnCalculated (IsExplicit: False)
///* ------------------ Example 2 ------------------ *///

--- Analyzing 'Square' for 'IShape' members ---

Interface Property: Area
  Corresponding Implementing Property: Square.Area (IsExplicit: False)
```

### example 2
#### extension methods
`InterfaceMapping.GetInfo` static method is defined in `InterfaceMappingExtensionMethods` static class.

`InterfaceMappingExtensionMethods.cs`

```
using System.Reflection;
using System.Text;

namespace Example.Extensions.ExtensionMethods.InterfaceMappingExtensionMethods
{
    public static class InterfaceMappingExtensionMethods
    {
        public static string GetInfo(
            this InterfaceMapping interfaceMapping   
        )
        {
            StringBuilder stringBuilder = new StringBuilder();

            stringBuilder.AppendFormat("Mapping of {0} to {1}:\n" , interfaceMapping.InterfaceType , interfaceMapping.TargetType);

            for(int ctr = 0; ctr < interfaceMapping.InterfaceMethods.Length; ctr++)
            {
                MethodInfo im = interfaceMapping.InterfaceMethods [ ctr ];
                MethodInfo tm = interfaceMapping.TargetMethods [ ctr ];
                stringBuilder.AppendFormat("   {0} --> {1}\n" , im.Name , tm.Name);
            }
            return stringBuilder.ToString();
        }
    }
}
```

#### main code
Invoking following method

```
       /// <summary>
        /// illustrate how to get interface mapping.
        /// </summary>
        public static void TestMethod36()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Type [ ] interfaceTypes = { typeof(IFormatProvider) , typeof(IAppDomainSetup) };
            Type [ ] implementationTypes = { typeof(CultureInfo) , typeof(AppDomainSetup) };
            for(int ctr = 0; ctr < interfaceTypes.Length; ctr++)
            {
                Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , ctr+1);
                Type interfaceType = interfaceTypes [ ctr ];
                Type implementationType = implementationTypes [ ctr ];
                InterfaceMapping map = implementationType.GetInterfaceMap(interfaceType);
                string textInfo = map.GetInfo();
                Console.WriteLine(textInfo);
                Console.WriteLine();
            }
        }
```

will following output

```
In TestMethod36 method call
///* ------------------ Example 1 ------------------ *///
Mapping of System.IFormatProvider to System.Globalization.CultureInfo:
   GetFormat --> GetFormat


///* ------------------ Example 2 ------------------ *///
Mapping of System.IAppDomainSetup to System.AppDomainSetup:
   get_ApplicationBase --> get_ApplicationBase
   set_ApplicationBase --> set_ApplicationBase
   get_ApplicationName --> get_ApplicationName
   set_ApplicationName --> set_ApplicationName
   get_CachePath --> get_CachePath
   set_CachePath --> set_CachePath
   get_ConfigurationFile --> get_ConfigurationFile
   set_ConfigurationFile --> set_ConfigurationFile
   get_DynamicBase --> get_DynamicBase
   set_DynamicBase --> set_DynamicBase
   get_LicenseFile --> get_LicenseFile
   set_LicenseFile --> set_LicenseFile
   get_PrivateBinPath --> get_PrivateBinPath
   set_PrivateBinPath --> set_PrivateBinPath
   get_PrivateBinPathProbe --> get_PrivateBinPathProbe
   set_PrivateBinPathProbe --> set_PrivateBinPathProbe
   get_ShadowCopyDirectories --> get_ShadowCopyDirectories
   set_ShadowCopyDirectories --> set_ShadowCopyDirectories
   get_ShadowCopyFiles --> get_ShadowCopyFiles
   set_ShadowCopyFiles --> set_ShadowCopyFiles

```
## reference
### API docs
+ [`Type.GetInterfaceMap(Type) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getinterfacemap?view=net-8.0)

### examples
+ The code of example 1 is modified from the example provided by [Google Gemini](https://g.co/gemini/share/fc1d0992d67f)

+ The code of example 2 is modified from the example in [`Type.GetInterfaceMap(Type) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getinterfacemap?view=net-8.0)