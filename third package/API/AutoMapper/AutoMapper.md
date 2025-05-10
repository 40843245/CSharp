# AutoMapper
## intro
AutoMapper is a useful package that easily converts it from an instance of class to another instance of class.

## How to create a mapping table
There are lots of ways.

### 1th way: create a mapping table with new `CreateMap` inside new `MapperConfiguration`
> [!CAUTION]
> This syntax is only supported for AutoMapper in version 5.0 (or above) 

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
    cfg =>
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

## How to specifies the behavior of mapping from one member of one class to other member of other class?
Invoke `ForMember` instance method followed by `CreateMap` method. 

```
        cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
```

For more details, see step 3 in above section.

## How to ignore the action (i.e. one member of one class will NOT to other member of other class)?
Invoke `Ignore` instance method in the lambda expression about action (which passed in 1th argument (zero-based)) inside `ForMember` instance method call.

```
            cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.birthdate, action => action.Ignore())
```

## How to use customized a value resolver to specify the action with complex operations?

Step 1:

To customize a value resolver, you need to define a class that implements `IValueResolver<TSource,TDestination,TReturnType>` interface.

By the definition of `IValueResolver<TSource,TDestination,TReturnType>` interface,

you will need to defines a method inside the class.

```
public TReturnType Resolve(TSource source, TDestination destination, TReturnType destMember, ResolutionContext context)`
{
    // your logic
}
```

Step 2:

Then one can invoke `ResolveUsing` instance method in the lambda expression about action 

with generic type `IValueResolver<TSource,TDestination,TReturnType>` inside `ForMember` instance method call.

For example,

I defined a class `AgeResolver` which implements `IValueResolver<User, UserDTO, int>` and define the Resolve method.

```
    public class AgeResolver : IValueResolver<User, UserDTO, int>
    {
        #region implement all interfaces
        public int Resolve(User source, UserDTO destination, int destMember, ResolutionContext context)
        {
            DateTime now = DateTime.Now;

            DateTime birthdate = source.birthdate;

            int age = now.Year - birthdate.Year;

            // Check if the birthday has occurred this year
            if (birthdate > now.AddYears(-age))
            {
                age--;
            }

            return age;
        }
        #endregion
    }
```

Then I can use it as follows.

```
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.AGE, action => action.ResolveUsing<AgeResolver>())
```

See example 2, for more details.

## 2th way: create and set up mapping table in `Profile` class (class that inherits `AutoMapper.Profile`) and add the profile class.

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
                cfg.CreateMap<User, UserDTO>();
                cfg.AddProfile<UserProfile>();

                cfg.CreateMap<UserDTO, User>();
                cfg.AddProfile<UserDTOProfile>();
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

## How to create a nested mapping table
Step 0:

Define two classes (of course, used for mapping table).

`InerSource.cs`

```
    public class InnerSource
    {
        public int OtherValue { get; set; }

    }
```

`OuterSource.cs`

```
    public class OuterSource
    {
        public int Value { get; set; }
        public InnerSource Inner { get; set; }
    }
```

Step 2:

Then create and setup mappint table.

‵InnerAndOuterMappers.cs`

```
    public class InnerAndOuterMappers
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<OuterSource, OuterDest>();
                cfg.CreateMap<InnerSource, InnerDest>();
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

Step 3:

Then we can use it

```
        public static void TestClass1()
        {
            var source = new OuterSource
            {
                Value = 5,
                Inner = new InnerSource { OtherValue = 15 }
            };
            var mapper = InnerAndOuterMappers.CreateMappingTable();
            var dest1 = mapper.Map<OuterSource, OuterDest>(source);
            string message = dest1.GetOuterDestInfo();
            Console.WriteLine(message);
        }
```
Then

## examples
### example 1
For example,

I have defined two classes.

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

Now, I want to convert an instance of `User` class (here, I name it as `user1`) to another instance of `UserDTO` class (here, I name it as `userDTO1`).

+ Without `AutoMapper` package, I have to access it from getter-setter property of `user1` and assign it to getter-setter property of `userDTO1`.

```
            User user1 = new User();
            user1.username = "Jay";
            user1.account = "jay30@gmail.com";
            user1.password = "password";

            UserDTO userDTO1 = new UserDTO();
            userDTO1.USERNAME = user1.username;
            userDTO1.ACCOUNT = user1.account;
            userDTO1.PASSWORD = user1.password;
```

It's easily to read, but it has fatal cons.
  
  - It's not flexible.
  - It's not to write code and hard to maintain.

Image that 

  - you have 1000 instance.
  - Or, you have an instance with 1000 getter-setter property.

With `AutoMapper` package (version 5.2.0), we just need to create mapping table through `CreateMap` method.

As following code.

To convert it from an instance of `User` class to another instance of `UserDTO` class, wrapping it into a method.

`UserDTOProfile.cs`

```
    public class UserDTOProfile
    {
        public static UserDTO GetUserDTO(User data)
        {
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
                   .ForMember(userDTO => userDTO.ACCOUNT, action => action.MapFrom(user => user.account))
                   .ForMember(userDTO => userDTO.PASSWORD, action => action.MapFrom(user => user.password))
                       ;
            });
            var mapper = config.CreateMapper();
            var result = mapper.Map<User,UserDTO>(data);
            return result;
        }
    }
```

Then we can use it

```
        public static void test2()
        {
            User user1 = new User();
            user1.username = "Jay";
            user1.account = "jay30@gmail.com";
            user1.password = "password";

            UserDTO userDTO1 = UserDTOProfile.GetUserDTO(user1);
            PrintInfo(user1, userDTO1);
        }
```

To convert it from an instance of `UserDTO` class to another instance of `User` class.

`UserProfile.cs`

```
    public class UserProfile
    {
        public static User GetUser(UserDTO data)
        {
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.username, action => action.MapFrom(userDTO => userDTO.USERNAME))
                   .ForMember(user => user.account, action => action.MapFrom(userDTO => userDTO.ACCOUNT))
                   .ForMember(user => user.password, action => action.MapFrom(userDTO => userDTO.PASSWORD))
                   ;

            });
            var mapper = config.CreateMapper();
            var result = mapper.Map<UserDTO,User>(data);
            return result;
        }
    }
```

Then we can use it

```
        public static void test3()
        {
            UserDTO userDTO1 = new UserDTO();
            userDTO1.USERNAME = "Jay";
            userDTO1.ACCOUNT = "jay30@gmail.com";
            userDTO1.PASSWORD = "password";

            User user1 = UserProfile.GetUser(userDTO1);
            PrintInfo(user1, userDTO1);
        }
```

Or, it can be even more simpler, we can create two mapping tables at same time and wrapping it into a method.

As following code

`UserMapper.cs`

```
    public class UserMapper
    {
        public static IMapper CrreateMappingTable()
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

To convert it from an instance of `User` class to another instance of `UserDTO` class, we can simply type

```
        public static void test4()
        {
            User user1 = new User();
            user1.username = "Jay";
            user1.account = "jay30@gmail.com";
            user1.password = "password";

            IMapper mapper = UserMapper.CrreateMappingTable();
            UserDTO userDTO1 = mapper.Map<User, UserDTO>(user1);
            PrintInfo(user1, userDTO1);
        }
```

To convert it from an instance of `UserDTO` class to another instance of `User` class.

```
        public static void test5()
        {
            UserDTO userDTO1 = new UserDTO();
            userDTO1.USERNAME = "Jay";
            userDTO1.ACCOUNT = "jay30@gmail.com";
            userDTO1.PASSWORD = "password";

            IMapper mapper = UserMapper.CrreateMappingTable();
            User user1 = mapper.Map<UserDTO, User>(userDTO1);
            PrintInfo(user1, userDTO1);
        }
```

where 

`PrintoInfo` method is defined as follows.

```
        private static void PrintInfo(User user, UserDTO userDTO)
        {
            Console.WriteLine(user.GetInfo());
            Console.WriteLine(userDTO.GetInfo());
            Console.ReadLine();
        }
```

#### demo project
See [`AutoMapper demo2.7z (version 1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/1.0.0)

### examples 2

`User.cs`

```
    public class User
    {
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

`UserMapper.cs`

```
    public class UserMapper
    {
        public static IMapper CrreateMappingTable()
        {
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
                   .ForMember(userDTO => userDTO.ACCOUNT, action => action.MapFrom(user => user.account))
                   .ForMember(userDTO => userDTO.PASSWORD, action => action.MapFrom(user => user.password))
                    .ForMember(userDTO => userDTO.AGE, action => action.ResolveUsing<AgeResolver>())

                       ;
                cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.username, action => action.MapFrom(userDTO => userDTO.USERNAME))
                   .ForMember(user => user.account, action => action.MapFrom(userDTO => userDTO.ACCOUNT))
                   .ForMember(user => user.password, action => action.MapFrom(userDTO => userDTO.PASSWORD))
                   .ForMember(user => user.birthdate, action => action.Ignore())
                   ;
            });
            var mapper = config.CreateMapper();
            return mapper;
        }
    }
```

`AgeResolver.cs`

```
    public class AgeResolver : IValueResolver<User, UserDTO, int>
    {
        #region implement all interfaces
        public int Resolve(User source, UserDTO destination, int destMember, ResolutionContext context)
        {
            DateTime now = DateTime.Now;

            DateTime birthdate = source.birthdate;

            int age = now.Year - birthdate.Year;

            // Check if the birthday has occurred this year
            if (birthdate > now.AddYears(-age))
            {
                age--;
            }

            return age;
        }
        #endregion
    }
```

`DemoClass1.cs`

```
public static class DemoClass1
    {
        /// <summary>
        /// static method that illustrate it's troublesome 
        /// to convert from a class to another class without `AutoMapper` package. 
        /// </summary>
        public static void test1()
        {
            User user1 = new User();
            user1.username = "Jay";
            user1.account = "jay30@gmail.com";
            user1.password = "password";

            UserDTO userDTO1 = new UserDTO();
            userDTO1.USERNAME = user1.username;
            userDTO1.ACCOUNT = user1.account;
            userDTO1.PASSWORD = user1.password;

            PrintInfo(user1, userDTO1);
        }

        public static void test4()
        {
            User user1 = new User();
            user1.username = "Jay";
            user1.account = "jay30@gmail.com";
            user1.password = "password";

            IMapper mapper = UserMapper.CrreateMappingTable();
            UserDTO userDTO1 = mapper.Map<User, UserDTO>(user1);
            PrintInfo(user1, userDTO1);
        }
        public static void test5()
        {
            UserDTO userDTO1 = new UserDTO();
            userDTO1.USERNAME = "Jay";
            userDTO1.ACCOUNT = "jay30@gmail.com";
            userDTO1.PASSWORD = "password";

            IMapper mapper = UserMapper.CrreateMappingTable();
            User user1 = mapper.Map<UserDTO, User>(userDTO1);
            PrintInfo(user1, userDTO1);
        }

        public static void test6()
        {
            User user1 = new User();
            user1.birthdate = new DateTime(2001, 1, 1);
            user1.username = "testuser";
            user1.account = "testaccount";
            user1.password = "testpassword";
            UserDTO userDTO1 = new UserDTO();
            userDTO1.AGE = 2;
            IMapper mapper = UserMapper.CrreateMappingTable();
            userDTO1 = mapper.Map<User, UserDTO>(user1);
            PrintInfo(user1, userDTO1);
        }


        private static void PrintInfo(User user, UserDTO userDTO)
        {
            Console.WriteLine(user.GetInfo());
            Console.WriteLine(userDTO.GetInfo());
            Console.ReadLine();
        }
    }
```

`ExtensionMethods.cs`, extension method defined in static class `ExtensionMethods`

```
    public static class ExtensionMethods
    {
        public static string GetInfo(this User user)
        {
            string result = 
                $"user.username:'{user.username}'\t" +
                $"user.account:'{user.account}'\t" +
                $"user.password:'{user.password}'\t" +
                $"user.birthdate:'{user.birthdate}'\t"
                ;
            return result;
        }

        public static string GetInfo(this UserDTO userDTO)
        {
            string result = 
                $"userDTO.USERNAME:'{userDTO.USERNAME}'\t" +
                $"userDTO.ACCOUNT:'{userDTO.ACCOUNT}'\t" +
                $"userDTO.PASSWORD:'{userDTO.PASSWORD}'\t" +
                $"userDTO.AGE:'{userDTO.AGE}'\t"
                ;
            return result;
        }
    }
```

#### demo project
See [`AutoMapper demo2.7z (version 2.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/2.0.0)

### example 3
#### demo project
See [`AutoMapper demo2.7z (version 3.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/3.0.0)

## reference
### API reference
See [latest version of AutoMapper API](https://docs.automapper.org/en/stable/)

### tutorial reference
See [AutoMapper —— 類別轉換超省力](https://igouist.github.io/post/2020/07/automapper/)
