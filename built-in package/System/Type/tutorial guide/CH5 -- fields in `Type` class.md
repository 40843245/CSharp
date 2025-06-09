# CH5 -- fields in `Type` class
## objectives
You will know 

+ fields in Type class.

## CH5.1 -- fields in `Type` class
### `Type.Delimiter` field
represents the delimiter that separates names in the namespace of the `Type`.

### `Type.EmptyTypes` field
represents an empty array of type `Type`.

### `Type.FilterAttribute` field
represents the member filter used on attributes.

### `Type.FilterName` field
represents the case-sensitive member filter used on names.

### `Type.FilterNameIgnoreCase` field
represents the case-insensitive member filter used on names.

### `Type.Missing` field
represents a missing value in the `Type` information. 

## examples
### example 1
Invoking following method

```
        /// <summary>
        ///  illustrate the fields of `Type` 
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            Console.WriteLine(Type.Delimiter);
            Console.WriteLine(Type.EmptyTypes);
            Console.WriteLine(Type.FilterAttribute);
            Console.WriteLine(Type.FilterName);
            Console.WriteLine(Type.FilterNameIgnoreCase);
            Console.WriteLine(Type.Missing);
        }
```

will output following

```
In TestMethod5 method call
.
System.Type[]
System.Reflection.MemberFilter
System.Reflection.MemberFilter
System.Reflection.MemberFilter
System.Reflection.Missing
```

## reference
### API docs
+ [`Type.Delimiter Field`](https://learn.microsoft.com/en-us/dotnet/api/system.type.delimiter?view=net-8.0)

+ [`Type.EmptyTypes Field`](https://learn.microsoft.com/en-us/dotnet/api/system.type.emptytypes?view=net-8.0)

+ [`Type.FilterAttribute Field`](https://learn.microsoft.com/en-us/dotnet/api/system.type.filterattribute?view=net-8.0)

+ [`Type.FilterName Field`](https://learn.microsoft.com/en-us/dotnet/api/system.type.filtername?view=net-8.0)

+ [`Type.FilterNameIgnoreCase Field`](https://learn.microsoft.com/en-us/dotnet/api/system.type.filternameignorecase?view=net-8.0)

+ [`Type.Missing Field`](https://learn.microsoft.com/en-us/dotnet/api/system.type.missing?view=net-8.0)