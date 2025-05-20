# CH2 -- creating a mapping table and set up the mapper configuration
## objectives 
In this article, you will learn how to

+ create a mapping table
+ set up the mapper configuration

**extra bonus**

you will also learn how to

+ mapping lots of members without repetitive loop.
+ create a reversed mapping table
+ create a nested mapping table

## CH2.1 -- create a mapping table
### 1th way: create a mapping table (using `CreateMap`) directly inside map configuration
> [!CAUTION]
> This syntax is ONLY supported for `AutoMapper` in version 5.0 (or above) 

Step 1:

Define two class (of course, as the source and destination of mapping table)

Suppose I define two classes -- `User` and `UserDTO`.

`User.cs`

```
    public class User
    {
        public string username { get; set; }
        public string account { get; set; }
        public string password { get; set; }
    }
```

`UserDTO.cs`

```
    public class UserDTO
    {
        public string USERNAME { get; set; }
        public string ACCOUNT { get; set; }
        public string PASSWORD { get; set; }
    }
```

Step 2:

First, to create and set up the mapper configuration, instantiate a `MapperConfiguration` instance by `new` keyword.  

```
var config = new MapperConfiguration(
    cfg => // ...
);
```

Step 3:

Then, to set up one or more mapping table, call `CreateMap` instance method in the lambda expression inside `MapperConfiguration` constructor.

To set the behavior of the mapping table (such as, the action of one member in one class maps to one member in another class), invoke `ForMember` instance method.  

```
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
                   .ForMember(userDTO => userDTO.ACCOUNT, action => action.MapFrom(user => user.account))
                   .ForMember(userDTO => userDTO.PASSWORD, action => action.MapFrom(user => user.password))
                       ;
                cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.username, action => action.MapFrom(userDTO => userDTO.USERNAME))
                   .ForMember(user => user.account, action => action.MapFrom(userDTO => userDTO.ACCOUNT))
                   .ForMember(user => user.password, action => action.MapFrom(userDTO => userDTO.PASSWORD))
                   ;
            });
```

Let's break down the above code snippet.

+ In
  
```
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
```

the member `USERNAME` of userDTO (which is type of `UserDTO`) will map to the member `username` of user (which is type of `User`) using default behavior (i.e. value of `USERNAME` equals to value of `username`)


Step 4:

To create the mapping table (`IMapper` type), just invoke `CreateMapper` method in cfg instance (with type `MapperConfiguration`).

```
            var config = new MapperConfiguration(cfg =>
            {
                // ... code about set up the table omitted for clarity
            });
            var mapper = config.CreateMapper();
```

Step 5:

The full code is

`User.cs`

```
    public class User
    {
        public string username { get; set; }
        public string account { get; set; }
        public string password { get; set; }
    }
```

`UserDTO.cs`

```
    public class UserDTO
    {
        public string USERNAME { get; set; }
        public string ACCOUNT { get; set; }
        public string PASSWORD { get; set; }
    }
```

```
    public class UserMapper
    {
        public static IMapper CreateMappingTable()
        {
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
                   .ForMember(userDTO => userDTO.ACCOUNT, action => action.MapFrom(user => user.account))
                   .ForMember(userDTO => userDTO.PASSWORD, action => action.MapFrom(user => user.password))
                       ;
                cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.username, action => action.MapFrom(userDTO => userDTO.USERNAME))
                   .ForMember(user => user.account, action => action.MapFrom(userDTO => userDTO.ACCOUNT))
                   .ForMember(user => user.password, action => action.MapFrom(userDTO => userDTO.PASSWORD))
                   ;
            });
            var mapper = config.CreateMapper();
            return mapper;
        }
    }
```

see example 1 for full code.

#### 2th way: create and set up mapping table (using `CreateMap`) in `Profile` classes (classes that inherits `AutoMapper.Profile`) and add the profile classes in mapper configuration one-by-one.

Step 1:

Define two class (of course, as the source and destination of mapping table)

Suppose I define two classes -- `User` and `UserDTO`.

`User.cs`

```
    public class User
    {
        // System.DateTime
        public DateTime birthdate { get; set; }
        public string username { get; set; }
        public string account { get; set; }
        public string password { get; set; }
    }
```

`UserDTO.cs`

```
    public class UserDTO
    {
        public int AGE { get; set; }
        public string USERNAME { get; set; }
        public string ACCOUNT { get; set; }
        public string PASSWORD { get; set; }
    }
```

Step 2:

> [!CAUTION]
> The following way to define profiles is only supported in AutoMapper in version 5.0.0 (or above).
>
> For older version, see [`Configuration`](https://docs.automapper.org/en/stable/Configuration.html)

Define profiles. 

Defines classes.

For each defined class, it MUST inherit `AutoMapper.Profile` class.

And create and set up a mapping table in the constructor definition.

`UserProfile.cs`

```
    public class UserProfile : AutoMapper.Profile
    {
        public UserProfile()
        {
            CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
                   .ForMember(userDTO => userDTO.ACCOUNT, action => action.MapFrom(user => user.account))
                   .ForMember(userDTO => userDTO.PASSWORD, action => action.MapFrom(user => user.password))
                   .ForMember(userDTO => userDTO.AGE, action => action.ResolveUsing<AgeResolver>())
                       ;
        }
    }
```

`UserDTOProfile.cs`

```
    public class UserDTOProfile : Profile
    {
        public UserDTOProfile()
        {
            CreateMap<UserDTO, User>()
                   .ForMember(user => user.username, action => action.MapFrom(userDTO => userDTO.USERNAME))
                   .ForMember(user => user.account, action => action.MapFrom(userDTO => userDTO.ACCOUNT))
                   .ForMember(user => user.password, action => action.MapFrom(userDTO => userDTO.PASSWORD))
                   .ForMember(user => user.birthdate, action => action.Ignore())
                   ;
        }
    }
```

Step 3:

Add profiles.

Next, instantiate a configuration. (same as step 4 in 1th way)

Then create mapping tables by invoking `CreateMapper` instance method of `MapperConfiguration`. (same as step 4 in 1th way)

```
    public class UserMappers
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.AddProfile<UserProfile>();
                cfg.AddProfile<UserDTOProfile>();
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

see example 2 for full code.

### 3th way -- create and set up mapping table in `Profile` class (class that inherits `AutoMapper.Profile`) and add the assemblies using (`AddMaps` instance method call).
> [!CAUTION]
> `AddMaps` instance method is ONLY supported in `AutoMapper` package in version 9.0.0 (or above)
>
> according to Microsoft Copilot's answer.
>
> <img width="617" alt="image" src="https://github.com/user-attachments/assets/52093760-24dc-4d19-95ab-67a881aa26e0" />
> 
> DONT believe the answers from [Google Gemini's answer](https://g.co/gemini/share/e9a6f05bebf8). **It's wrong**

Step 1 and Step 2 are same to that in 2th way.

Step 3:

Add assemblies

```
        var configuration = new MapperConfiguration(cfg => {
                cfg.AddMaps(
                    new[]
                    {
                        typeof(Mapping.MapperProfiles.UserProfile).Assembly,
                        typeof(Mapping.MapperProfiles.UserDTOProfile).Assembly,
                    }
                );
            });
```

see example 3 for full code.

## CH2.3 -- mapping lots of members without repetitive loop.
One does NOT need to mapping lots of members with repetitive loop.

To do that, just simply invoke `Map` instance method of `mapper` instance (`IMapper` data type) with generic type `TDestination`.

Thanks to `AutoMapper`, due to this feature

+ When creating a mapping table (using `AutoMapper.Create<TSource,TDestination>`), it will auto generate create these mapping tables including

   - `AutoMapper.Create<IEnumerable<TSource>,IEnumerable<TDestination>>`
   - `AutoMapper.Create<TSource[],TDestination[]>`
   - etc
  
> [!IMPORTANT]
> In `AutoMapper`, `TSource` source collection types supports including
> - `IEnumerable`
> - `IEnumerable<T>`
> - `ICollection`
> - `ICollection<T>`
> - `IList`
> - `IList<T>`
> - `List<T>`
> - `Arrays`
>
> For more details, see [`Lists and Arrays`](https://docs.automapper.org/en/stable/Lists-and-arrays.html`)

For example,

Instead of 

```
// source is `List<OuterSource>` type.

List<OuterDest> listDest = new List<OuterDest>();
foreach(source in sources)
{
    var dest = mapper.Map<OuterSource,OuterDest>(source);
    listDest.Add(dest);
}
```

We can simply use one mapping table.

```
// source is `List<OuterSource>` type.

List<OuterDest> listDest = mapper.Map<List<OuterSource>,List<OuterDest>>(sources);
```

Take code snippets in above section for example.

Invoking this static method

```
        public static void TestClass2()
        {
            var sources = new OuterSource[]
            {
                new OuterSource
                {
                    Value = 5,
                    Inner = new InnerSource { OtherValue = 15 }
                },
                new OuterSource
                {
                    Value = 15,
                    Inner = new InnerSource { OtherValue = 15 }
                }
            };
            var mapper = InnerAndOuterMappers.CreateMappingTable();
            IEnumerable<OuterDest> iEnumerableDest = mapper.Map<OuterSource[], IEnumerable<OuterDest>>(sources);
            foreach (var dest1 in iEnumerableDest)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
            ICollection<OuterDest> iCollectionDest = mapper.Map<OuterSource[], ICollection<OuterDest>>(sources);
            foreach (var dest1 in iCollectionDest)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
            IList<OuterDest> iListDest = mapper.Map<OuterSource[], IList<OuterDest>>(sources);
            foreach (var dest1 in iListDest)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
            List<OuterDest> listDest = mapper.Map<OuterSource[], List<OuterDest>>(sources);
            foreach (var dest1 in listDest)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
            OuterDest[] DestArray = mapper.Map<OuterSource[], OuterDest[]>(sources);
            foreach (var dest1 in DestArray)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
        }
```

will output following in console.

```
outerDest.Value:5
innerDest.OtherValue:15

outerDest.Value:15
innerDest.OtherValue:15

outerDest.Value:5
innerDest.OtherValue:15

outerDest.Value:15
innerDest.OtherValue:15

outerDest.Value:5
innerDest.OtherValue:15

outerDest.Value:15
innerDest.OtherValue:15

outerDest.Value:5
innerDest.OtherValue:15

outerDest.Value:15
innerDest.OtherValue:15

outerDest.Value:5
innerDest.OtherValue:15

outerDest.Value:15
innerDest.OtherValue:15

```

## CH2.4 -- create a reversed mapping table
Chain `ReverseMap` instance method of `CreateMap` type.

```
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<Order, OrderDto>()
                   .ReverseMap();
            });
```

see example 4 for full code.

For more details, see [Reverse Mapping and Unflattening](https://docs.automapper.org/en/stable/Reverse-Mapping-and-Unflattening.html) 


## examples
### example 1
#### demo project
See [`AutoMapper demo2.7z (version 1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/1.0.0)
run code snippets in example 1 for fully understanding.

### example 2
#### demo project
See [`AutoMapper demo2.7z (version 3.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/3.0.0/AutoMapper%20demo2.7z)

### example 3
#### demo project
See [`AutoMapper demo2.7z (version 4.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/4.0.0/AutoMapper%20demo2.7z)

### example 4
#### demo project
See [`AutoMapper demo4.7z (version 1.0.0)`](https://github.com/40843245/CSharp/blob/main/third%20package/API/AutoMapper/AutoMapper%20demo4/1.0.0/AutoMapper%20demo4.7z)

## reference
### further reading
+ [`Getting Started Guide`](https://docs.automapper.org/en/stable/Getting-started.html)
+ [`Lists and Arrays`](https://docs.automapper.org/en/stable/Lists-and-arrays.html`)
+ [`Reverse Mapping and Unflattening`](https://docs.automapper.org/en/stable/Reverse-Mapping-and-Unflattening.html) 
