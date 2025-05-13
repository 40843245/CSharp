# CH1 -- intro
## objectives
You will know 

+ What is `AutoMapper`
+ pro of creating mapping tables with `AutoMapper` package
  
## CH1.1 -- intro
`AutoMapper` is a useful package that easily converts it from an instance of class to another instance of class.

## CH1.2 -- pro of creating mapping tables with `AutoMapper` package
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

## examples
### example 1
#### demo project
See [`AutoMapper demo2.7z (version 1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/1.0.0)
run code snippets in example 1 for fully understanding.1
