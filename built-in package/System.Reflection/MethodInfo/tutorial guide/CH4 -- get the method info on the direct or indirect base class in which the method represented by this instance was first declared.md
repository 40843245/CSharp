# CH4 -- get the method info on the direct or indirect base class in which the method represented by this instance was first declared
## objectives
You will learn how to

+ get the method info on the direct or indirect base class in which the method represented by this instance was first declared

## CH4.1 -- get the method info on the direct or indirect base class in which the method represented by this instance was first declared
### `MethodInfo.GetBaseDefinition` instance method
When overridden in a derived class, get the method info on the direct or indirect base class in which the method represented by this instance was first declared.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get the method info on the direct or indirect base class in which the method represented by this instance was first declared
        /// </summary>
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Type t = typeof(DerivedClass);
            MethodInfo m, mb;
            string [ ] methodNames = { "ToString", "Equals", "InterfaceImpl",
                               "Method1", "Method2", "Method3" };

            foreach(var methodName in methodNames)
            {
                m = t.GetMethod(methodName);
                mb = m.GetBaseDefinition();
                Console.WriteLine("{0}.{1} --> {2}.{3}" , m.ReflectedType.Name ,
                                  m.Name , mb.ReflectedType.Name , mb.Name);
            }
        }
```

will output following

```
In TestMethod4 method call,
DerivedClass.ToString --> Object.ToString
DerivedClass.Equals --> Object.Equals
DerivedClass.InterfaceImpl --> DerivedClass.InterfaceImpl
DerivedClass.Method1 --> BaseClass.Method1
DerivedClass.Method2 --> BaseClass.Method2
DerivedClass.Method3 --> DerivedClass.Method3
```

## reference
### API docs
+ [`MethodInfo.GetBaseDefinition Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.getbasedefinition?view=net-8.0)

+ [`MethodInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo?view=net-8.0)

### examples
+ The code of example 1 is referenced from example in [`MethodInfo.GetBaseDefinition Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo.getbasedefinition?view=net-8.0)
