# CH30 -- get type code
## objectives
You will learn how to

+ get type code given `Type` instance

## CH30.1 -- get type code given `Type` instance
### `Type.GetTypeCode` static method
Given `Type` instance, 

the `Type.GetTypeCode` static method will return its correpsonding type code (`TypeCode` Enum type).

## examples
### example 1
Invoking following method 

```
        /// <summary>
        /// illustrate how to get typeCode given a `Type` instance through `Type.GetTypeCode` static method
        /// </summary>
        public static void TestMethod21()
        {
            Type [ ] datas = new Type [ ]
            {
                null,
                typeof(object),
                typeof(bool),
                typeof(byte),
                typeof(short),
                typeof(int),
                typeof(long),
                typeof(decimal),
                typeof(ushort),
                typeof(uint),
                typeof(ulong),
                typeof(char),
                typeof(string),
                typeof(Exception),
                typeof(List<int>),
                typeof(HashSet<int>),
                typeof(KeyValuePair<int,string>),
                typeof(Dictionary<int,string>),
                typeof(IList<int>),
                typeof(ICollection<int>),
                typeof(System.Console),
                typeof(Person),
                typeof(Employee),
                typeof(GenericRepository<Person>),
                typeof(EmployeeRepository),
                typeof(IGenericRepository<Person>),
                typeof(GenericService),
                typeof(IndentationHandler),
                new List<int>().GetType(),
                (new int[]{ }).GetType(),
            };
                       
            foreach(var data in datas)
            {
                TypeCode typeCode = Type.GetTypeCode(data);
                Console.WriteLine("The {0} type has typeCode:{1}" , data?.FullName ?? "null" , (int)typeCode) ;
            }
        }
```

will output following

```
The null type has typeCode:0
The System.Object type has typeCode:1
The System.Boolean type has typeCode:3
The System.Byte type has typeCode:6
The System.Int16 type has typeCode:7
The System.Int32 type has typeCode:9
The System.Int64 type has typeCode:11
The System.Decimal type has typeCode:15
The System.UInt16 type has typeCode:8
The System.UInt32 type has typeCode:10
The System.UInt64 type has typeCode:12
The System.Char type has typeCode:4
The System.String type has typeCode:18
The System.Exception type has typeCode:1
The System.Collections.Generic.List`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Collections.Generic.HashSet`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Collections.Generic.KeyValuePair`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Collections.Generic.Dictionary`2[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089],[System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Collections.Generic.IList`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Collections.Generic.ICollection`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Console type has typeCode:1
The Example.Bean.Person type has typeCode:1
The Example.DirtyBean.Employee type has typeCode:1
The Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type has typeCode:1
The Example.Repositories.EmployeeRepository type has typeCode:1
The Example.Generics.Repository.IGenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type has typeCode:1
The Example.Generics.Service.GenericService type has typeCode:1
The Example.Helpers.Indentation.IndentationHandler type has typeCode:1
The System.Collections.Generic.List`1[[System.Int32, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089]] type has typeCode:1
The System.Int32[] type has typeCode:1
```

## reference
### API docs
+ [`Type.GetTypeCode(Type) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.gettypecode?view=net-8.0)
+ [`TypeCode Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.typecode?view=net-8.0)