# AutoMapper
## intro
`AutoMapper` is a useful package that easily converts it from an instance of class to another instance of class.

## pro of creating mapping tables with `AutoMapper` package
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

run code snippets in example 1 for fully understanding.

## creating a mapping table
### How to create a mapping table
There are lots of ways.

#### 1th way: create a mapping table (using `CreateMap`) directly inside map configuration
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

see example 1.

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

see example 3.

### 3th way -- create and set up mapping table in `Profile` class (class that inherits `AutoMapper.Profile`) and add the assemblies using (`AddMaps` instance method call).
> [!CAUTION]
> `AddMaps` instance method is ONLY supported in `AutoMapper` package in version 9.0.0 (or above)
>
> according to Microsoft Copilot's answer.
>
> <img width="617" alt="image" src="https://github.com/user-attachments/assets/52093760-24dc-4d19-95ab-67a881aa26e0" />
> 
> DONT believe the answers from [Google Gemini's answer](https://g.co/gemini/share/e9a6f05bebf8). **It's wrong**

## create a nested mapping table
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

see example 4.

### How to map lots of elements using mapping table?
Just simply invoke `Map` instance method of `mapper` instance (`IMapper` data type) with generic type `TDestination`.

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
> For more details, see (Lists and Arrays)[`https://docs.automapper.org/en/stable/Lists-and-arrays.html`]

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

### How to create a reverse mapping table?
Chain `ReverseMap` instance method of `CreateMap` type.

```
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<Order, OrderDto>()
                   .ReverseMap();
            });
```

For more details, see [Reverse Mapping and Unflattening](https://docs.automapper.org/en/stable/Reverse-Mapping-and-Unflattening.html) 

See example 5. for more understanding.

## specifies the action
### How to specifies the action of mapping from one member of one class to other member of other class?
Invoke `ForMember` instance method followed by `CreateMap` method. 

```
        cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.USERNAME, action => action.MapFrom(user => user.username))
```

For more details, see step 3 in above section.

### How to ignore the action (i.e. one member of one class will NOT to other member of other class)?
Invoke `Ignore` instance method in the lambda expression about action (which passed in 1th argument (zero-based)) inside `ForMember` instance method call.

```
            cfg.CreateMap<UserDTO, User>()
                   .ForMember(user => user.birthdate, action => action.Ignore())
```

## value resolver
### How to use customized a value resolver to specify the action with complex operations?

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
> [!CAUTION]
> `ResolveUsing` is deprecated in `AutoMapper` package in version 8.0.0.
>
> For later version, use `MapFrom` instead, the syntax of `MapFrom` is equivalent to that of `ResolveUsing`.
>
> For example,
> 
> ```
> cfg.ForMember(userDTO => userDTO.AGE, action => action.MapFrom<AgeResolver>())
> ```

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

## value transformer
### value transformer at global level
If one create a value transformer at global level, the value transformer will apply to all members of class in the map configuration.

```
             var configuration = new MapperConfiguration(cfg => {
                // ... add profiles or assemblies

                // create global value transformer for strings that will trim (the space) from the string 
                cfg.ValueTransformers.Add<string>(str => str.Trim());
            });
```

Take example 10 for example,

`Question.cs`

```
    public class Question
    {
        public User Asker { get; set; }

        public string Title { get; set; }

        public string Body { get; set; }

        public List<Answer> Responses { get; set; }
    }
```

`QuestionDto.cs`

```
    [AutoMap(typeof(Question))]
    public class QuestionDto
    {
        public User Asker { get; set; }

        public string Title { get; set; }

        public string Body { get; set; }

        public List<Answer> Responses { get; set; }

    }
```

`QuestionProfile.cs`

```
using AutoMapper;
// ...

    public class QuestionProfile : Profile
    {
        public QuestionProfile()
        {
            CreateMap<Question, QuestionDto>();
        }
    }
```

`MappingTableConfiguration.cs`

```
    public class MappingTableConfiguration
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.AddProfile<QuestionProfile>();

                // ... other profiles omitted for clarity.

                // Global value transformer for strings
                cfg.ValueTransformers.Add<string>(str => str.Trim());
            });

            IMapper mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

Then invoke the following static method

```
        public static void TestMethod1()
        {
            Question[] questions = new Question[]
            {
                new Question()
                {
                    Asker = new User()
                    {
                        firstName = "Yazawa",
                        lastName = "Nico"
                    },
                    Title = "Can 'Yazawa Nico' join the LoveLive club?",
                    Body = "I love music and dancing. So I want to join LoveLive club.",
                    Responses = new List<Answer>()
                },
                new Question()
                {
                    Asker = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Title = "How to manage the LoveLive club?",
                    Body = " "+"I'm a student council in LoveLive school.\nI'm encountering a very huge difficulty.\nThere are fewer and fewer student want to join in our school.\nHow can I do?" + " ",
                    Responses = new List<Answer>()
                }
            };

            IMapper mapper = MappingTableConfiguration.CreateMappingTable();
            QuestionDto[]  questionDtos = mapper.Map<QuestionDto[]>(questions);

            Console.WriteLine("TestMethod1");
            foreach (var questionDto in questionDtos)
            {
                Console.WriteLine(questionDto.GetQuestionDtoInfo());
            }
            Console.WriteLine("------------------------------------------------------");
        }
```

will output following in console.

```
TestMethod1
'Yazawa Nico' asks a question.
Title:Can 'Yazawa Nico' join the LoveLive club?
Body:I love music and dancing. So I want to join LoveLive club.

'Ayase Eli' asks a question.
Title:How to manage the LoveLive club?
Body:I'm a student council in LoveLive school.
I'm encountering a very huge difficulty.
There are fewer and fewer student want to join in our school.
How can I do?

------------------------------------------------------
```

<img width="867" alt="image" src="https://github.com/user-attachments/assets/2bce76f8-3d61-45c0-94c1-c0f5eb9f7492" />

you will found there are exactly one whitespace at the begin and end of the `Body` getter-setter property

```
Body = " "+"I'm a student council in LoveLive school.\nI'm encountering a very huge difficulty.\nThere are fewer and fewer student want to join in our school.\nHow can I do?" + " ",
```

But, it does NOT output the leading and trailing whitespaces. 

That's the following global value transformer does.

```
cfg.ValueTransformers.Add<string>(str => str.Trim());
```

Run code snippets of example 10 for more understanding.

### value transformer at member level
If one create a value transformer at member level, the value transformer will ONLY apply to the members of the class.

```
    public class QuestionProfile : Profile
    {
        public QuestionProfile()
        {
            CreateMap<Question, QuestionDto>()
                /// create member-level value transformer
                .ForMember(questionDto => questionDto.Body, action => action.AddTransform(val => "Hello everone!!! " + val));
                ;
        }
    }
```

Take preceeding example for example,

Modifies `QuestionProfile.cs` as followings.

```
    public class QuestionProfile : Profile
    {
        public QuestionProfile()
        {
            CreateMap<Question, QuestionDto>()
                /// create member-level value transformer
                .ForMember(questionDto => questionDto.Body, action => action.AddTransform(val => "Hello everone!!! " + val));
                ;
        }
    }
```

Then invoking the method `TestMethod1` (defined in above example)

will output following in console.

```
TestMethod1
'Yazawa Nico' asks a question.
Title:Can 'Yazawa Nico' join the LoveLive club?
Body:Hello everone!!! I love music and dancing. So I want to join LoveLive club.

'Ayase Eli' asks a question.
Title:How to manage the LoveLive club?
Body:Hello everone!!!  I'm a student council in LoveLive school.
I'm encountering a very huge difficulty.
There are fewer and fewer student want to join in our school.
How can I do?

------------------------------------------------------
```

you will found the `Body` uses blockquotes `Hello Everyone!!!` at the begin.

That's value transformer of member level does

```
.ForMember(questionDto => questionDto.Body, action => action.AddTransform(val => "Hello everone!!! " + val));
```

Run code snippets in example 10 for more fully understanding.

### value transformer at map level
If one create a value transformer at map level, the value transformer will ONLY apply to the members of all the mapping table (whose type of member that saitisfies the generic type).

```
        public QuestionProfile()
        {
            CreateMap<Question, QuestionDto>()
                // create a value transformer in map level.
                .AddTransform<string>(val =>"LoveLive School Question."+val)
                ;
        }
```

Take preceeding example for example.

Modifies the `QuestionProfile.cs` 

```
    public class QuestionProfile : Profile
    {
        public QuestionProfile()
        {
            CreateMap<Question, QuestionDto>()
                // create a value transformer in map level.
                .AddTransform<string>(val =>"LoveLive School Question."+val)
                ;
        }
    }
```

And, here, it will add new string `LoveLive School Question.` at the begin of the member whose type is string and defines in this mapper. 

Then invoking method TestMethod1,

it will output the following in console

```
TestMethod1
'Yazawa Nico' asks a question.
Title:LoveLive School Question.Can 'Yazawa Nico' join the LoveLive club?
Body:LoveLive School Question.I love music and dancing. So I want to join LoveLive club.

'Ayase Eli' asks a question.
Title:LoveLive School Question.How to manage the LoveLive club?
Body:LoveLive School Question. I'm a student council in LoveLive school.
I'm encountering a very huge difficulty.
There are fewer and fewer student want to join in our school.
How can I do?

------------------------------------------------------
```

<img width="853" alt="image" src="https://github.com/user-attachments/assets/b25c1015-b954-4682-bfb9-b56029e25ea6" />

you will found that in Title (string` type) and Body (string` type) BOTH added blockquote `LoveLive School Question.`

Run code snippets in example 10 for more fully understanding.

### value transformer at profile level
If one create a value transformer at profile level, the value transformer will apply to the members of the mapping tables (whose type of member that saitisfies the generic type) in the Profile class.

```
    public class AnswerProfile : Profile
    {
        public AnswerProfile()
        {
            CreateMap<Answer, AnswerDto>();
            // create a value transformer in profile level.
            ValueTransformers.Add<string>(val=> $"Answered at:{DateTime.Now.ToString(@"MM\/dd\/yyyy HH:mm")},{val}");
        }
    }
```

Take preceeding example for example.

Add Entity

`Answer.cs`

```
    public class Answer
    {
        public User Answerer { get; set; }

        public string Title { get; set; }

        public string Body { get; set; }
    }
```

Then add DTO

`AnswerDto.cs`

```
    [AutoMap(typeof(Question))]
    public class AnswerDto
    {
        public User Answerer { get; set; }

        public string Title { get; set; }

        public string Body { get; set; }
    }
```

Then add profile.

`AnswerProfile.cs`

```
    public class AnswerProfile : Profile
    {
        public AnswerProfile()
        {
            CreateMap<Answer, AnswerDto>();
            // create a value transformer in profile level.
            ValueTransformers.Add<string>(val=> $"Answered at:{DateTime.Now.ToString(@"MM\/dd\/yyyy HH:mm")},{val}");
        }
    }
```

Then invoke the following method 

```
        public static void TestMethod2()
        {
            Question[] questions = new Question[]
            {
                new Question()
                {
                    Asker = new User()
                    {
                        firstName = "Yazawa",
                        lastName = "Nico"
                    },
                    Title = "Can 'Yazawa Nico' join the LoveLive club?",
                    Body = "I love music and dancing. So I want to join LoveLive club.",
                    Responses = new List<Answer>()
                },
                new Question()
                {
                    Asker = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Title = "How to manage the LoveLive club?",
                    Body = " "+"I'm a student council in LoveLive school.\nI'm encountering a very huge difficulty.\nThere are fewer and fewer student want to join in our school.\nHow can I do?" + " ",
                    Responses = new List<Answer>()
                }
            };

            Answer[] answers = new Answer[]
            {
                new Answer()
                {
                    Answerer = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Title = "Brief answer",
                    Body = "Yes, you can join the LoveLive club.",
                },
                new Answer()
                {
                    Answerer = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Title = "Detailed answer",
                    Body = " "+"Get first rank on the following dancing contest."+" ",
                }   
            };

            questions[0].Responses.Add(answers[0]);
            questions[1].Responses.Add(answers[1]);

            IMapper mapper = MappingTableConfiguration.CreateMappingTable();
            QuestionDto[] questionDtos = mapper.Map<QuestionDto[]>(questions);
            AnswerDto[] answerDtos = mapper.Map<AnswerDto[]>(answers);

            Console.WriteLine("TestMethod2");
            foreach (var questionDto in questionDtos)
            {
                Console.WriteLine(questionDto.GetQuestionDtoInfo());
            }
            Console.WriteLine("------------------------------------------------------");
            foreach (var answerDto in answerDtos)
            {
                Console.WriteLine(answerDto.GetAnswerDtoInfo());
            }
            Console.WriteLine("------------------------------------------------------");

        }
```

will output following in console.

```
TestMethod2
'Yazawa Nico' asks a question.
Title:LoveLive School Question.Can 'Yazawa Nico' join the LoveLive club?
Body:LoveLive School Question.I love music and dancing. So I want to join LoveLive club.

'Ayase Eli' asks a question.
Title:LoveLive School Question.How to manage the LoveLive club?
Body:LoveLive School Question. I'm a student council in LoveLive school.
I'm encountering a very huge difficulty.
There are fewer and fewer student want to join in our school.
How can I do?

------------------------------------------------------
'Ayase Eli' answers a question.
Title:Answered at:05/13/2025 11:11,Brief answer
Body:Answered at:05/13/2025 11:11,Yes, you can join the LoveLive club.

'Ayase Eli' answers a question.
Title:Answered at:05/13/2025 11:11,Detailed answer
Body:Answered at:05/13/2025 11:11, Get first rank on the following dancing contest.

------------------------------------------------------
```

<img width="856" alt="image" src="https://github.com/user-attachments/assets/4a8ce8b6-e03c-4c56-8d1b-96919c9a746a" />

Run code snippets in example 10 for more understanding.

## mapping table with conditions
### How to do conditional mapping?
Invoke `Condition` instance method in the lambda expression about action (which passed in 1th argument (zero-based)) inside `ForMember` instance method call.

```
        var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<OuterSource, OuterDest>()
                    .ForMember(
                        outerSource => outerSource.Value, 
                        action => 
                        {
                            action.Condition(outerDest=>outerDest.Value >= 0);
                            action.MapFrom(outerDest => outerDest.Value);
                        }
                    )
                    ;
        });
```

Modify the class `InnerAndOuterMappers` as follows.

```
    public class InnerAndOuterMappers
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<OuterSource, OuterDest>()
                    .ForMember(
                        outerSource => outerSource.Value, 
                        action => 
                        {
                            action.Condition(outerDest=>outerDest.Value >= 0);
                            action.MapFrom(outerDest => outerDest.Value);
                        }
                    )
                    ;
                cfg.CreateMap<InnerSource, InnerDest>()
                    .ForMember(
                        innerSource => innerSource.OtherValue,
                        action =>
                        {
                            action.Condition(innerDest => innerDest.OtherValue >= 0);
                            action.MapFrom(innerDest => innerDest.OtherValue);
                        }
                    )
                    ;
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

Then invoking the following method will

```
        public static void TestClass3()
        {
            var outerSourceArray = new OuterSource[]
            {
                new OuterSource
                {
                    Value = 10,
                    Inner = new InnerSource { OtherValue = 5 }
                },
                new OuterSource
                {
                    Value = 0,
                    Inner = new InnerSource { OtherValue = 0 }
                },
                new OuterSource
                {
                    Value = -2,
                    Inner = new InnerSource { OtherValue = -3 }
                }
            };

            var innerSourceArray = new InnerSource[]
            {
                new InnerSource
                {
                    OtherValue = 10
                },
                new InnerSource
                {
                    OtherValue = 0
                },
                new InnerSource
                {
                    OtherValue = -2
                }
            };

            var mapper = InnerAndOuterMappers.CreateMappingTable();

            OuterDest[] outerDestArray = mapper.Map<OuterSource[], OuterDest[]>(outerSourceArray);
            foreach (var dest1 in outerDestArray)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
            Console.WriteLine("--------------------------------------------");

            InnerDest[] innerDestArray = mapper.Map<InnerSource[], InnerDest[]>(innerSourceArray);
            foreach (var dest1 in innerDestArray)
            {
                string message = dest1.GetInnerDestInfo();
                Console.WriteLine(message);
            }
            Console.WriteLine("--------------------------------------------");
        }
```

will output following in console.

```
outerDest.Value:10
innerDest.OtherValue:5

outerDest.Value:0
innerDest.OtherValue:0

outerDest.Value:0
innerDest.OtherValue:0

--------------------------------------------
innerDest.OtherValue:10

innerDest.OtherValue:0

innerDest.OtherValue:0

--------------------------------------------
```

For more details, see the docs - [Conditional Mapping](https://docs.automapper.org/en/stable/Conditional-mapping.html#conditional-mapping) 

### How to do pre-conditional mapping?
Invoke `PreCondition` instance method in the lambda expression about action (which passed in 1th argument (zero-based)) inside `ForMember` instance method call.

```
        var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<OuterSource, OuterDest>()
                    .ForMember(
                        outerSource => outerSource.Value, 
                        action => 
                        {
                            action.PreCondition(outerDest=>outerDest.Value >= 0);
                            action.MapFrom(outerDest => outerDest.Value);
                        }
                    )
                    ;
        });
```

Take code snippets in above section for example.

Modify the class `InnerAndOuterMappers` as follows.

```
    public class InnerAndOuterMappers
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<OuterSource, OuterDest>()
                    .ForMember(
                        outerSource => outerSource.Value, 
                        action => 
                        {
                            action.PreCondition(outerDest=>outerDest.Value >= 0);
                            action.MapFrom(outerDest => outerDest.Value);
                        }
                    )
                    ;
                cfg.CreateMap<InnerSource, InnerDest>()
                    .ForMember(
                        innerSource => innerSource.OtherValue,
                        action =>
                        {
                            action.PreCondition(innerDest => innerDest.OtherValue >= 0);
                            action.MapFrom(innerDest => innerDest.OtherValue);
                        }
                    )
                    ;
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

Then invoking the static method

```
        public static void TestClass4()
        {
            var outerSourceArray = new OuterSource[]
            {
                new OuterSource
                {
                    Value = 10,
                    Inner = new InnerSource { OtherValue = 5 }
                },
                new OuterSource
                {
                    Value = 0,
                    Inner = new InnerSource { OtherValue = 0 }
                },
                new OuterSource
                {
                    Value = -2,
                    Inner = new InnerSource { OtherValue = -3 }
                }
            };

            var innerSourceArray = new InnerSource[]
            {
                new InnerSource
                {
                    OtherValue = 10
                },
                new InnerSource
                {
                    OtherValue = 0
                },
                new InnerSource
                {
                    OtherValue = -2
                }
            };

            var mapper = InnerAndOuterMappers.CreateMappingTable();

            OuterDest[] outerDestArray = mapper.Map<OuterSource[], OuterDest[]>(outerSourceArray);
            foreach (var dest1 in outerDestArray)
            {
                string message = dest1.GetOuterDestInfo();
                Console.WriteLine(message);
            }
            Console.WriteLine("--------------------------------------------");

            InnerDest[] innerDestArray = mapper.Map<InnerSource[], InnerDest[]>(innerSourceArray);
            foreach (var dest1 in innerDestArray)
            {
                string message = dest1.GetInnerDestInfo();
                Console.WriteLine(message);
            }
            Console.WriteLine("--------------------------------------------");
        }
```

will output following in console.

```
outerDest.Value:10
innerDest.OtherValue:5

outerDest.Value:0
innerDest.OtherValue:0

outerDest.Value:0
innerDest.OtherValue:0

--------------------------------------------
innerDest.OtherValue:10

innerDest.OtherValue:0

innerDest.OtherValue:0

--------------------------------------------
```

For more details, see the docs - [Preconditions](https://docs.automapper.org/en/stable/Conditional-mapping.html#preconditions) 

## null substitution
### How to substitution from value of member of class which is null to specific value.
Of course, you can use value Resolver that implements `IValueResolver` interface and defines `Resolve` with following logic.

1. Check whether the value of member of class which is null or not.
2. If it is null, then returns the specific value.
3. If it is not null, then returns it.  

However, you can invoke `NullSubstitute` instance method in the lambda expression about action (which passed in 1th argument (zero-based)) inside `ForMember` instance method call.

```
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<InnerSource, InnerDest>()
                    .ForMember(
                        innerSource => innerSource.OtherValue,
                        action =>
                        {
                            action.NullSubstitute(2);
                        }
                    )
                    ;
            });
```

Take code snippets in above section for example.

Modify the class `InnerAndOuterMappers` as follows.

```
    public class InnerAndOuterMappers
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<OuterSource, OuterDest>()
                   .ForMember(
                        outerSource => outerSource.Value,
                        action =>
                        {
                            action.Condition(outerDest => outerDest.Value >= 0);
                            action.MapFrom(outerDest => outerDest.Value);
                        }
                    )
                    ;
                cfg.CreateMap<InnerSource, InnerDest>()
                    .ForMember(
                        innerSource => innerSource.OtherValue,
                        action =>
                        {
                            action.NullSubstitute(2);
                        }
                    )
                    ;
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

Then invoking the method will

```
        public static void TestClass5()
        {
            var innerSourceArray = new InnerSource[]
            {
                new InnerSource
                {
                    OtherValue = 50
                },
                new InnerSource
                {
                    OtherValue = 0
                },
                new InnerSource
                {
                    OtherValue = null
                }
            };

          
            var mapper = InnerAndOuterMappers.CreateMappingTable();
            InnerDest[] innerDestArray = mapper.Map<InnerSource[], InnerDest[]>(innerSourceArray);
            foreach (var dest1 in innerDestArray)
            {
                string message = dest1.GetInnerDestInfo();
                Console.WriteLine(message);
            }
            Console.WriteLine("--------------------------------------------");
        }
```

will output following in console.

```
innerDest.OtherValue:50

innerDest.OtherValue:0

innerDest.OtherValue:2

--------------------------------------------
```

see example 6.

## Mapping inheritence
### configuring inheritance from the base class
You can invoke `Include` instance method to include the derived class that will be used for mapping table.

For example:

`Order.cs`

```
public class Order { }
public class OnlineOrder : Order { }
public class MailOrder : Order { }
```

`OrderDto.cs`

```
public class OrderDto { }
public class OnlineOrderDto : OrderDto { }
public class MailOrderDto : OrderDto { }
```

`OrderMappingTable.cs`

```
   public class OrderMappingTable
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<Order, OrderDto>()
                    .Include<OnlineOrder, OnlineOrderDto>()
                    .Include<MailOrder, MailOrderDto>();
                cfg.CreateMap<OnlineOrder, OnlineOrderDto>();
                cfg.CreateMap<MailOrder, MailOrderDto>();
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

Then `mapped` defined in this statement `var mapped = mapper.Map(onlineOrder, onlineOrder.GetType(), typeof(OrderDto));` is type of `OnlineOrderDto`

`DemoClass1.cs`

```
using Xunit;
// ...

    public static class DemoClass1
    {
        public static void TestMethod1()
        {
            var onlineOrder = new OnlineOrder();
            var mapper = OrderMappingTable.CreateMappingTable();
            var mapped = mapper.Map(onlineOrder, onlineOrder.GetType(), typeof(OrderDto));
            Assert.IsType<OnlineOrderDto>(mapped);  // will NOT throw exceptions.
        }
    }
```

For more details, see [Runtime polymorphism](https://docs.automapper.org/en/stable/Mapping-inheritance.html#runtime-polymorphism) 

See example 6. for more understanding.

### specifying inheritance in derived classes
Instead of configuring inheritance from the base class, you can specify inheritance from the derived classes:

For example:

add getter-setter property in `Order` class.

`Order.cs`

```
public class Order
{
    public int OrderId {get;set;} 
}
public class OnlineOrder : Order { }
public class MailOrder : Order { }
```

And add getter-setter property in `OrderDto` class.

`OrderDto.cs`

```
public class OrderDto 
{
    public int Id { get; set; }
}
public class OnlineOrderDto : OrderDto { }
public class MailOrderDto : OrderDto { }
```

The modify the `OrderMappingTable.CreateMappingTable` static method as follows:

`OrderMappingTable.cs`

```
   public class OrderMappingTable
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<Order, OrderDto>()
                  .ForMember(o => o.Id, m => m.MapFrom(s => s.OrderId));
                cfg.CreateMap<OnlineOrder, OnlineOrderDto>()
                  .IncludeBase<Order, OrderDto>();
                cfg.CreateMap<MailOrder, MailOrderDto>()
                  .IncludeBase<Order, OrderDto>();
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

For more details, see [Specifying inheritance in derived classes](https://docs.automapper.org/en/stable/Mapping-inheritance.html#runtime-polymorphism) 

See example 7. for more understanding.

### redirect a base map to an existing derived map.
For simple cases, you can use As to redirect a base map to an existing derived map

```
    cfg.CreateMap<Order, OnlineOrderDto>();
    cfg.CreateMap<Order, OrderDto>().As<OnlineOrderDto>();
    
    mapper.Map<OrderDto>(new Order()).ShouldBeOfType<OnlineOrderDto>();
```

### Inheritance Mapping Priorities

+ Explicit Mapping (using `MapFrom` instance method)
+ Inherited Explicit Mapping
+ Ignore Property Mapping (using `Ignore` instance method)
+ Convention Mapping (Properties that are matched via convention)

To demonstrate this, lets modify our classes.

`Order.cs`

```
    public class Order { }

    public class OnlineOrder : Order
    {
        public string Referrer { get; set; }
    }
    public class MailOrder : Order { }
```

`OrderDto.cs`

```
    public class OrderDto
    {
        public string Referrer { get; set; }
    }
```

`OrderMappingTable.cs`

```
     public class OrderMappingTable
    {
        public static IMapper CreateMappingTable()
        {
            var configuration = new MapperConfiguration(cfg => {
                cfg.CreateMap<Order, OrderDto>()
                    .Include<OnlineOrder, OrderDto>()
                    .Include<MailOrder, OrderDto>()
                    .ForMember(o => o.Referrer, m => m.Ignore());
                cfg.CreateMap<OnlineOrder, OrderDto>();
                cfg.CreateMap<MailOrder, OrderDto>();
            });
            var mapper = configuration.CreateMapper();
            return mapper;
        }
    }
```

```
using Xunit;
// ...

    public static class DemoClass1
    {
        public static void TestMethod1()
        {
            var order = new OnlineOrder { Referrer = "google" };
            IMapper mapper = OrderMappingTable.CreateMappingTable();
            var mapped = (OnlineOrder)mapper.Map(order, order.GetType(), typeof(OrderDto));
            Assert.Null(mapped.Referrer);
        }
    }
```

Run code snippets in example 8, for more fully understanding.

## attribute mapping
### declare an class with `AutoMap`
> [!CAUTION]
> `AutoMapAttribute` is ONLY supported in `AutoMapper` package in version 8.1.0 (or above)
>
> The answers references [Google Gemini's answer](https://g.co/gemini/share/f56e364f1cae).

To declare an attribute map, decorate your destination class with `AutoMapAttribute` (which can be shorten as `AutoMap`).

The basic structure should look like this:

`Order.cs`

```
// source class
public class Order {
    // source members
}
```

`OrderDto.cs`

```
// destination class
[AutoMap(typeof(Order))]
public class OrderDto {
    // destination members
}
```

Image that one wants to map from all members of source class to all members of destination class, 

one can chain `ForMember` instance method ono-by-one for all members in source class. It's so troublesome.

At that time, `AutoMap` helps a lots.

For those members with same name, `AutoMap` tells compiler to automatically map from to the source members to the desintaion members.

And thus, you don't have to explicitly map (using `.ForMember` instance method) from to the source members to the desintaion members, for those members with same name, if the destination class is marked with `AutoMapAttribute`.

For example,

`Order.cs`

```
    /// source class
    public class Order
    {
        public int Id { get; set; }
        public User OrderPlacer { get; set; }
    }
```

`User.cs`

```
    public class User
    {
        public string firstName { get; set; }
        public string lastName { get; set; }
    }
```

`OrderDto.cs`

```
    /// destination class
    [AutoMap(typeof(Order))]
    public class OrderDto
    {
        public int Id { get; set; }

        public User OrderPlacer { get; set; }

    }
```

Without `AutoMap`, or say if the `destination class` -- `OrderDto` class is NOT marked as `AutoMap` with `[AutoMap(typeof(Order))]` decoration, as following defintion.

```
    /// destination class
    public class OrderDto
    {
        public int Id { get; set; }

        public User OrderPlacer { get; set; }
    }
```

one needs to explicitly map for each members,

and might write the following code snippet.

```
            var configuration = new MapperConfiguration(cfg =>
            {
                /// method declaration with generics -- `CreateMap<TSource, TDestination>()`
                cfg.CreateMap<Order, OrderDto>()
                    .ForMember(orderDto => orderDto.Id, action => action.MapFrom(order=>order.Id))
                    .ForMember(orderDto => orderDto.OrderPlacer, action => action.MapFrom(order=>order.OrderPlacer))
            });
```

It's so troublesome.

With `AutoMap`, or say if the `destination class` -- `OrderDto` class is marked as `AutoMap` with `[AutoMap(typeof(Order))]` decoration, as following defintion.

```
    /// destination class
    [AutoMap(typeof(Order))]
    public class OrderDto
    {
        public int Id { get; set; }

        public User OrderPlacer { get; set; }

    }
```

one don't have to explicitly map for those members with same name,

and can be simplified as

```
            var configuration = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<Order, OrderDto>();
            });
```

For full code, see example 9.

Run code snippets in example 9, for more fully understanding.

## examples
### example 1
#### demo project
See [`AutoMapper demo2.7z (version 1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/1.0.0)

### examples 2
#### demo project
See [`AutoMapper demo2.7z (version 2.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/2.0.0/AutoMapper%20demo2.7z)

### example 3
#### demo project
See [`AutoMapper demo2.7z (version 3.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/3.0.0/AutoMapper%20demo2.7z)

### example 4
#### demo project
See [`AutoMapper demo2.7z (version 4.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/4.0.0/AutoMapper%20demo2.7z)

### example 5
#### demo project
See [`AutoMapper demo4.7z (version (1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo4/1.0.0/AutoMapper%20demo4.7z)

### example 6
#### demo project
See [`AutoMapper demo5.7z (version (1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo5/1.0.0/AutoMapper%20demo5.7z)

### example 7
#### demo project
See [`AutoMapper demo5.7z (version (2.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo5/2.0.0/AutoMapper%20demo5.7z)

### example 8
#### demo project
See [`AutoMapper demo5.7z (version (3.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo5/3.0.0/AutoMapper%20demo5.7z)

### example 9
#### demo project
See [`AutoMapper demo6.7z (version (1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo6/1.0.0/AutoMapper%20demo6.7z)

### example 10
#### demo project
See [`AutoMapper demo7.7z (version (4.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo7/4.0.0/AutoMapper%20demo7.7z)

## reference
### API reference
See [latest version of AutoMapper API](https://docs.automapper.org/en/stable/)

### tutorial reference
See [AutoMapper —— 類別轉換超省力](https://igouist.github.io/post/2020/07/automapper/)
