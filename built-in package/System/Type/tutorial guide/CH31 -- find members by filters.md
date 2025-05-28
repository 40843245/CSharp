# CH31 -- find members by filters
## objectives
You will learn how to 

+ find members by filters

## CH31.1 -- find members by filters
### `FindMembers` instance method
#### Syntax

```
public virtual System.Reflection.MemberInfo[] FindMembers
(
    System.Reflection.MemberTypes memberType, 
    System.Reflection.BindingFlags bindingAttr, 
    System.Reflection.MemberFilter? filter, 
    object? filterCriteria
);
```

##### Parameters
+ `memberType`: is a flag (bitwise operation of enum value) that indicates the type of member to search for.
+ `bindingAttr`: is a flag that specifies how the search is conducted.
+ `filter`: A delegate that filters the data. 

If ,for the current element, the delegate returns true, it will NOT be discarded (and thus it is an element of returned value of `FindMembers`). 

Otherwise, it will be discarded.

+ `filterCriteria`: The search criteria that determines whether a member is returned in the array of MemberInfo objects.

##### Returned value
`MemberInfo[]` type containing some members that matches the search.

Iff no members matches the search, it will return an empty array of `MemberInfo`.

The search is deterimined by these four arguments.

#### Remarks
> [!CAUTION]
> You must specify `BindingFlags.Instance` or `BindingFlags.Static` along with `BindingFlags.Public` or `BindingFlags.NonPublic`.
> 
> Otherwise, no members will be match.

## examples
### example 1
#### extension method
see demo project for complete code.

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to filter specific members through `FindMembers` instance method call.
        /// </summary>
        public static void TestMethod22()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            string textInfo;
            MemberTypes memberTypes;
            MemberTypes [ ] memberTypeses;
            BindingFlags bindingFlags;
            BindingFlags [ ] bindingFlagses;
            MemberFilter memberFilter;
            object? filterCiteria;
            MemberInfo [ ] memberInfos;
            int counter = 1;

            ///* --------------- Example 1 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///",counter);
            Employee personKotori = new Employee
            {
                FirstName = "Minami" ,
                LastName = "Kotori" ,
            };

            /// filtered member will 
            /// + be a property
            memberTypes = 
                MemberTypes.Property
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property
            };

            /// filtered member will 
            /// + be from an instance
            /// + be public
            /// + be getter property and
            /// + be setter property
            bindingFlags = 
                BindingFlags.Instance |
                BindingFlags.Public | 
                BindingFlags.GetProperty |
                BindingFlags.SetProperty 
                ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
                BindingFlags.Public,
                BindingFlags.GetProperty,
                BindingFlags.SetProperty,
            };

            memberFilter = null;

            filterCiteria = null;

            Type personKotoriType = personKotori.GetType();

            memberInfos = personKotoriType.FindMembers(
                memberTypes,
                bindingFlags,
                memberFilter,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                personKotoriType,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* --------------- Example 2 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);
            Employee employeeNico = new Employee
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
                Company = new Company()
                {
                    Id = 1 ,
                    Name = "LoveLive club" ,
                }
            };
            Type employeeNicoType = employeeNico.GetType();
            
            /// filtered member will 
            /// + be a property and 
            /// + be nested type 
            memberTypes = 
                MemberTypes.Property |
                MemberTypes.NestedType
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property,
                 MemberTypes.NestedType
            };

            /// filtered member will 
            /// + be from an instance
            /// + be NOT public
            /// + be getter property and
            /// + be setter property
            bindingFlags =
               BindingFlags.Instance |
               BindingFlags.NonPublic |
               BindingFlags.GetProperty |
               BindingFlags.SetProperty
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
                BindingFlags.NonPublic,
                BindingFlags.GetProperty,
                BindingFlags.SetProperty,
            };

            memberFilter = null;

            filterCiteria = null;

            memberInfos = employeeNicoType.FindMembers(
                memberTypes ,
                bindingFlags,
                memberFilter,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                employeeNicoType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* --------------- Example 3 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);
            Employee employeeEli = new Employee
            {
                FirstName = "Ayase" ,
                LastName = "Eli" ,
                Company = new Company()
                {
                    Id = 1 ,
                    Name = "LoveLive club" ,
                }
            };
            Type employeeEliType = employeeEli.GetType();

            /// filtered member will 
            /// + be a property and 
            memberTypes =
                MemberTypes.Property
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property,
            };

            /// filtered member will 
            /// + be from an instance
            /// + be public
            /// + be in the class at top level (i.e. inherited members are not considered.)
            /// + be getter property and
            /// + be setter property
            bindingFlags =
               BindingFlags.Instance |
               BindingFlags.Public |
               BindingFlags.DeclaredOnly |
               BindingFlags.GetProperty |
               BindingFlags.SetProperty 
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
                BindingFlags.Public,
                BindingFlags.DeclaredOnly,
                BindingFlags.GetProperty,
                BindingFlags.SetProperty,
            };

            memberFilter = null;

            filterCiteria = null;

            memberInfos = employeeEliType.FindMembers(
                memberTypes ,
                bindingFlags ,
                memberFilter ,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                employeeEliType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;


            ///* --------------- Example 4 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            Type flagsesExtensionMethodsType = typeof(FlagsesExtensionMethods);

            /// filtered member will 
            /// + be a method 
            memberTypes =
                MemberTypes.Method
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Method,
            };

            /// filtered member will 
            /// + be from a static class
            /// + be public
            /// + be getter property and
            /// + be setter property
            bindingFlags =
               BindingFlags.Static |
               BindingFlags.Public |
               BindingFlags.GetProperty |
               BindingFlags.SetProperty
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Static,
                BindingFlags.Public,
                BindingFlags.GetProperty,
                BindingFlags.SetProperty,
            };

            memberFilter = null;

            filterCiteria = null;

            memberInfos = flagsesExtensionMethodsType.FindMembers(
                memberTypes ,
                bindingFlags ,
                memberFilter ,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                flagsesExtensionMethodsType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* --------------- Example 5 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            Employee employeeHonoka = new Employee()
            {
                FirstName = "Kosaka" ,
                LastName = "Honoka" ,
                Company = new Company()
                {
                    Id = 1 ,
                    Name = "LoveLive club" ,
                }
            };

            Type employeeHonokaType = employeeHonoka.GetType();
            /// filtered member will 
            /// + be a property
            /// + be custom (i.e. NOT a primitive data type)
            memberTypes =
                MemberTypes.Property
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property,
            };

            /// filtered member will 
            /// + be from an instance
            /// + be public
            /// + be getter property 
            bindingFlags =
               BindingFlags.Instance |
               BindingFlags.Public |
               BindingFlags.GetProperty
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
                BindingFlags.Public,
                BindingFlags.GetProperty,
            };

            memberFilter = null;

            filterCiteria = null;

            memberInfos = employeeHonokaType.FindMembers(
                memberTypes ,
                bindingFlags ,
                memberFilter ,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                employeeHonokaType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* --------------- Example 6 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            Company loveLiveSchool = new Company()
            {
                Id = 1,
                Name = "LoveLive club" ,
            };

            Type loveLiveSchoolType = loveLiveSchool.GetType();

            /// filtered member will 
            /// + be a property
            /// + be custom (i.e. NOT a primitive data type)
            memberTypes =
                MemberTypes.Property |
                MemberTypes.Custom
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property,
                 MemberTypes.Custom,
            };

            /// filtered member will 
            /// + be from an instance
            /// + be public
            /// + be getter property 
            bindingFlags =
               BindingFlags.Instance |
               BindingFlags.Public |
               BindingFlags.GetProperty
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
                BindingFlags.Public,
                BindingFlags.GetProperty,
            };

            memberFilter = null;

            filterCiteria = null;

            memberInfos = loveLiveSchoolType.FindMembers(
                memberTypes ,
                bindingFlags ,
                memberFilter ,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                loveLiveSchoolType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* --------------- Example 7 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            Employee employeeUmi = new Employee()
            {        
                LastName  = "Umi" ,
            };

            Type employeeUmiType = employeeUmi.GetType();

            /// filtered member will 
            /// + be a property
            memberTypes =
                MemberTypes.Property
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property,
            };

            /// filtered member will 
            /// + be from an instance
            bindingFlags =
               BindingFlags.Instance
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
            };

            memberFilter = null;

            filterCiteria = null;

            memberInfos = employeeUmiType.FindMembers(
                memberTypes ,
                bindingFlags ,
                memberFilter ,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                employeeUmiType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* --------------- Example 8 --------------- *///
            Console.WriteLine(@"///* --------------- Example {0} --------------- *///" , counter);

            Employee employeeRin = new Employee() // 星空凜 Hoshizora Rin
            {
                LastName = "Rin",
            };

            Type employeeRinType = employeeRin.GetType();

            /// filtered member will 
            /// + be a property
            memberTypes =
                MemberTypes.Property
                ;

            memberTypeses = new MemberTypes [ ]
            {
                 MemberTypes.Property,
            };

            /// filtered member will 
            /// + be from an instance
            bindingFlags =
               BindingFlags.Instance
               ;

            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.Instance,
            };

            // filter by filter value of getter setter property of `LastName` in `employeeUmi` instance does NOT start with `U` (which is always evaluated to false)
            memberFilter = 
                (mInfo,citeria) => {
                    return !employeeUmi.LastName.StartsWith("U");
                };

            filterCiteria = null;

            memberInfos = employeeUmiType.FindMembers(
                memberTypes ,
                bindingFlags ,
                memberFilter ,
                filterCiteria
            );

            textInfo = memberInfos.GetInfo(
                employeeUmiType ,
                memberTypeses ,
                bindingFlagses ,
                memberFilter ,
                filterCiteria
            );

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod22 method call
///* --------------- Example 1 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance|BindingFlags.Public|BindingFlags.GetProperty|BindingFlags.SetProperty
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are 3 such member(s) in our search in `Example.DirtyBean.Employee` type.
0th memberInfo.Name:Company
1th memberInfo.Name:FirstName
2th memberInfo.Name:LastName

///* --------------- Example 2 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property|MemberTypes.NestedType
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance|BindingFlags.NonPublic|BindingFlags.GetProperty|BindingFlags.SetProperty
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are no such member(s) in our search in `Example.DirtyBean.Employee` type.

///* --------------- Example 3 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance|BindingFlags.Public|BindingFlags.DeclaredOnly|BindingFlags.GetProperty|BindingFlags.SetProperty
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are 1 such member(s) in our search in `Example.DirtyBean.Employee` type.
0th memberInfo.Name:Company

///* --------------- Example 4 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Method
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Static|BindingFlags.Public|BindingFlags.GetProperty|BindingFlags.SetProperty
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are 1 such member(s) in our search in `Example.Extensions.ExtensionMethods.FlagsesExtensionMethods.FlagsesExtensionMethods` type.
0th memberInfo.Name:GetInfo

///* --------------- Example 5 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance|BindingFlags.Public|BindingFlags.GetProperty
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are 3 such member(s) in our search in `Example.DirtyBean.Employee` type.
0th memberInfo.Name:Company
1th memberInfo.Name:FirstName
2th memberInfo.Name:LastName

///* --------------- Example 6 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property|MemberTypes.Custom
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance|BindingFlags.Public|BindingFlags.GetProperty
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are 2 such member(s) in our search in `Example.Bean.Company` type.
0th memberInfo.Name:Id
1th memberInfo.Name:Name

///* --------------- Example 7 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance
Filtered with the `MemberFilter` delegate:
null
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are no such member(s) in our search in `Example.DirtyBean.Employee` type.

///* --------------- Example 8 --------------- *///
Filtered with these `MemberTypes` enum (which marked as `System.Flag`):
MemberTypes.Property
Filtered with these `BindingFlags` enum (which marked as `System.Flag`):
BindingFlags.Instance
Filtered with the `MemberFilter` delegate:
System.Reflection.MemberFilter
Filtered with the `filterCiteria` (which is nullable `object` type):
null

There are no such member(s) in our search in `Example.DirtyBean.Employee` type.

```

## reference
### API docs
+ [`Type.FindMembers(MemberTypes, BindingFlags, MemberFilter, Object) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.findmembers?view=net-9.0)

+ about flags, [`MemberTypes Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.membertypes?view=net-9.0)

+ about flags, [`BindingFlags Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.bindingflags?view=net-9.0)

+ about delegate, [`MemberFilter Delegate`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberfilter?view=net-9.0)