# CH8 -- value transformer
## objectives
In this article, you will learn how to

+ specify a value transformer

## CH8.1 -- specify a value transformer
### value transformer at global level
If one create a value transformer at global level, the value transformer will apply to all members of class in the map configuration.

```
             var configuration = new MapperConfiguration(cfg => {
                // ... add profiles or assemblies

                // create global value transformer for strings that will trim (the space) from the string 
                cfg.ValueTransformers.Add<string>(str => str.Trim());
            });
```

For example,

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

see example 1 for full code.

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

see example 1 for full code.

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

see example 1 for full code.

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

see example 1 for full code.

## examples
### example 1
#### demo project
See [`AutoMapper demo7.7z (version 4.0.0)`](https://github.com/40843245/CSharp/blob/main/third%20package/API/AutoMapper/AutoMapper%20demo7/4.0.0/AutoMapper%20demo7.7z)

## reference
### further reading
+ [`Value Transformers`](https://docs.automapper.org/en/stable/Value-transformers.html)
