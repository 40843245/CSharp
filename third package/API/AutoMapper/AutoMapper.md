# AutoMapper
## intro
AutoMapper is a useful package that easily converts it from an instance of class to another instance of class.

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

## examples 

I have define two class

`personnel_train_brower.cs`

```
public partial class personnel_train_brower
    {
        public personnel_train_brower()
        {
            this.personnel_train_brower_attach = new HashSet<personnel_train_brower_attach>();
        }
    
        public int PERSONNEL_ID { get; set; }
        public string PERSONNEL_TITILE { get; set; }
        public System.DateTime PERSONNEL_TIME { get; set; }
        public string PERSONNEL_FILE_PATH { get; set; }
        public int CREATE_USER { get; set; }
        public System.DateTime CREATE_TIME { get; set; }
        public string READ_PERMISSION { get; set; }
        public string PERSONNEL_CATEGORY_ID { get; set; }
        public Nullable<int> CLICK_NUM { get; set; }
    
        public virtual ICollection<personnel_train_brower_attach> personnel_train_brower_attach { get; set; }
    }
}
```

`PersonnelTrainBrower.cs`

```
    public class PersonnelTrainBrower
    {

        public string BrowerId { get; set; }

        [Display(Name = "人事訓練名稱:")]
        [Required(ErrorMessage = "必須填寫.")]
        public string BrowerTitle { get; set; }

        [Display(Name = "人事訓練日期:")]
        [Required(ErrorMessage = "必須填寫.")]
        public DateTime BrowerTime { get; set; }

        [Display(Name = "人事訓練檔案:")]
        public string BrowerFilePath { get; set; }

        [Display(Name = "人事訓練分類:")]
        [JsonIgnore]
        public string BrowerCategoryIdStr { get; set; }

        public List<string> BrowerCategoryIdList { get; set; }


        [JsonIgnore]
        [Display(Name = "人事訓練檔案上傳:")]
        public HttpPostedFileBase UploadFile { get; set; }

        [JsonIgnore]
        [Display(Name = "附件:")]
        public List<HttpPostedFileBase> UploadFile2 { get; set; }

        public List<PersonnelTrainBrowerAttach> FileList { get; set; }

        public List<string> HistoryList { get; set; }

        public List<string> SelfList { get; set; }
        public string BrowerFileName { get; set; }

        public string BrowerFormatTime { get; set; }

        public string BrowerDownloadFile { get; set; }

        [Display(Name = "相關連結")]
        public string BrowerUrl { get; set; }

        [Display(Name = "相關連結名稱")]
        public string BrowerUrlName { get; set; }

        [Required(ErrorMessage = "請設定閱讀權限")]
        [Display(Name = "閱讀權限:")]
        [JsonIgnore]
        public string ReadPermission { get; set; }


        public Nullable<int> CLICK_NUM { get; set; }

    }
```

To convert it from an instance of `personnel_train_brower` class to another instance of `PersonnelTrainBrower` class and

to convert it from an instance of `PersonnelTrainBrower` class to another instance of `personnel_train_brower` class.

With `AutoMapper` package (version <= 5.0.0) (obsolete), we have to create two mapping tables through `AutoMapper.Mapper.CreateMap` static method in overridden `Configure` method.

As following code.

```
    public class PersonnelTrainBrowerProfile : BaseProfile
    {
        protected override void Configure()
        {
            Mapper.CreateMap<personnel_train_brower, PersonnelTrainBrower>()
                .ForMember(dest => dest.BrowerId, opt => opt.ResolveUsing<ParameterEncodebyDicResolver>().FromMember(src => new Dictionary<string, string> { { Constants.EncyptId, src.PERSONNEL_ID.ToString() } }))
                .ForMember(dest => dest.BrowerTime, opt => opt.MapFrom(src => src.PERSONNEL_TIME))
                .ForMember(dest => dest.BrowerTitle, opt => opt.MapFrom(src => src.PERSONNEL_TITILE))
                .ForMember(dest => dest.BrowerFilePath, opt => opt.MapFrom(src => src.PERSONNEL_FILE_PATH))
                .ForMember(dest => dest.BrowerFileName, opt => opt.MapFrom(src => Path.GetFileName(src.PERSONNEL_FILE_PATH)))
                .ForMember(dest => dest.BrowerFormatTime, opt => opt.MapFrom(src => src.PERSONNEL_TIME.ToString("yyyy/MM/dd")))
                .ForMember(dest => dest.BrowerDownloadFile, opt => opt.MapFrom(src => String.IsNullOrEmpty(src.PERSONNEL_FILE_PATH) ? "" : VirtualPathUtility.ToAbsolute(src.PERSONNEL_FILE_PATH)))
                //.ForMember(dest => dest.BrowerUrl,opt=>opt.MapFrom(src=>src.PERSONNEL_URL))
                //.ForMember(dest => dest.BrowerUrlName, opt => opt.MapFrom(src => src.PERSONNEL_URL_NAME))
                .ForMember(dest => dest.FileList, opt => opt.MapFrom(src => src.personnel_train_brower_attach))
                .ForMember(dest => dest.ReadPermission, opt => opt.MapFrom(src => String.IsNullOrEmpty(src.READ_PERMISSION) ? src.READ_PERMISSION : src.READ_PERMISSION.Substring(1, src.READ_PERMISSION.Length - 2)))
                .ForMember(dest => dest.BrowerCategoryIdStr, opt => opt.MapFrom(src => src.PERSONNEL_CATEGORY_ID))
             ;
            Mapper.CreateMap<PersonnelTrainBrower, personnel_train_brower>()
                .ForMember(dest => dest.PERSONNEL_ID, opt => opt.Ignore())
                .ForMember(dest => dest.PERSONNEL_TIME, opt => opt.MapFrom(src => src.BrowerTime))
                .ForMember(dest => dest.PERSONNEL_TITILE, opt => opt.MapFrom(src => src.BrowerTitle))
                .ForMember(dest => dest.PERSONNEL_FILE_PATH, opt => opt.MapFrom(src => src.BrowerFilePath))
                //.ForMember(dest => dest.PERSONNEL_URL, opt => opt.MapFrom(src => src.BrowerUrl))
                //.ForMember(dest => dest.PERSONNEL_URL_NAME, opt => opt.MapFrom(src => src.BrowerUrlName))
                .ForMember(dest => dest.PERSONNEL_CATEGORY_ID, option => option.MapFrom(b => String.IsNullOrEmpty(b.BrowerCategoryIdStr) ? String.Empty : String.Format(",{0},", b.BrowerCategoryIdStr)))
                .ForMember(dest => dest.READ_PERMISSION, opt => opt.MapFrom(src => String.IsNullOrEmpty(src.ReadPermission) ? String.Empty : String.Format(",{0},", src.ReadPermission)))
                .ForMember(dest => dest.CREATE_USER, opt =>
                {
                    opt.Condition(resolutionContext =>
                        (resolutionContext.InstanceCache.First().Value as personnel_train_brower).PERSONNEL_ID == 0);
                    opt.MapFrom(src => CoreUtility.GetUserCoockieData().USER_ID);
                })
                .ForMember(dest => dest.CREATE_TIME, opt =>
                {
                    opt.Condition(resolutionContext =>
                        (resolutionContext.InstanceCache.First().Value as personnel_train_brower).PERSONNEL_ID == 0);
                    opt.MapFrom(src => DateTime.Now);
                });
        }
    }
```

where

`BaseProfile` class is defined as follows.

`BaseProfile.cs`

```
using AutoMapper;

    public class BaseProfile : Profile
    {
        public sealed override string ProfileName
        {
            get
            {
                return this.GetType().Name;
            }
        }        
    }
```

`Profile` class is defined in `AutoMapper` namespace.

To use the mapping table, we can simply type.

```
personnel_train_brower entity = new personnel_train_brower();
// ...
// ... set `personnel_train_brower` getter-setter property here.
// ...

PersonnelTrainBrower bean = new PersonnelTrainBrower();
Mapper.Map(entity, personnelBrower);
```
## demo project
See [`AutoMapper demo2 (version 5.2.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper)

## reference
### API reference
See [latest version of AutoMapper API](https://docs.automapper.org/en/stable/)

### tutorial reference
See [AutoMapper —— 類別轉換超省力](https://igouist.github.io/post/2020/07/automapper/)
