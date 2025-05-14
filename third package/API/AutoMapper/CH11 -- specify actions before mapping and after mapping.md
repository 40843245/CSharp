# CH11 -- specify actions before mapping and after mapping
## objectives
In this article, you will learn how to

+ specify actions before mapping (`BeforeMap`)
+ specify actions after mapping (`AfterMap`)
+ specify actions before mapping or after mapping with template (`Process` method in `IMappingAction` interface)

**extra bonus**

you will know its pros.

## CH11.1 -- specify actions before mapping (`BeforeMap`)
To map from preprocessed data, we can preprocess data before mapping then mapping them (using such as `mapper.Map<TSource,TDestination>` instance method call).

However, it's so troublesome.

In recent version of `AutoMapper`, we can simply chain `.BeforeMap` method just after creating the mapping table (such as `AutoMapper.CreateMap<TSource,TDestination>`).

```
public class StudentInfoProfile: Profile
    {
        public StudentInfoProfile()
        {
            // CreateMap<TSource, TDestination>
            CreateMap<StudentInfo, StudentInfoDto>()
                .BeforeMap((studentInfo, studentInfoDto) => // type of the parameter in the lambda expression is (TSource,TDestination)
                    // ... logic to preprocess data before mapping them.
                );
        }
    }
```

For example,

`StudentInfo.cs`

```
    public class StudentInfo
    {
        public User Student { get; set; }
        public int Score { get; set; }
    }
```

`StudentInfoDto.cs`

```
using AutoMapper;
// ...

    [AutoMap(typeof(StudentInfo))]
    public class StudentInfoDto
    {
        public User Student { get; set; }
        public int Score { get; set; }
    }
```

Here are the code snippets about logic to preprocessing data.

`INumberHandlerBase.cs` which defines the interface.

```
    public interface INumberHandlerBase
    {
        int Adjust(StudentInfo studentInfo, int adjustmentFactor);
    }
```

`GeometricAdjustificationHandler.cs` which specifies how to preprocessing data.

```
    public class GeometricAdjustificationHandler: INumberHandlerBase
    {
        private int _upperLimit;
        private int _lowerLimit;

        public GeometricAdjustificationHandler(
            int upperLimit,
            int lowerLimit
        )
        {
            Swapper.TryToSwap(ref upperLimit, ref lowerLimit);
            this._upperLimit = upperLimit;
            this._lowerLimit = lowerLimit;
        }
        public int Adjust(StudentInfo studentInfo, int adjustmentFactor)
        {
            int newScore = studentInfo.Score * adjustmentFactor;
            newScore = Clamper.Clamp(newScore,this._lowerLimit,this._upperLimit);
            return newScore;
        }
    }
```

Here are code snippets of its utility class.

`Swapper.cs`

```
    public static class Swapper
    {
        #region Swap two numbers
        public static void TryToSwap(
            ref int upperBound,
            ref int lowerBound
        )
        {
            if (lowerBound > upperBound)
            {
                Swapper.Swap(ref upperBound, ref lowerBound);
            }
        }

        public static void Swap(
            ref int source,
            ref int dest
        )
        {
            int temp = source;
            source = dest;
            dest = temp;
        }
        #endregion
    }
```

`Clamper.cs`

```
    public static class Clamper
    {
        public static T Clamp<T>(T value, T min, T max)
            where T : System.IComparable<T>
        {
            T result = value;
            if (value.CompareTo(max) > 0)
            {
                result = max;
            }
            if (value.CompareTo(min) < 0)
            {
                result = min;
            }
            return result;
        }
    }
```

When invoking the following test method:

```
        public static void TestMethod1()
        {
            StudentInfo[] studentInfos = new StudentInfo[]
            {
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Yazawa",
                        lastName = "Nico"
                    },
                    Score = 30
                },
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Score = 60
                },
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Minami",
                        lastName = "Kotori"
                    },
                    Score = 20
                },
            };


            Console.WriteLine("Before adjusting the score,");
            Console.WriteLine();
            Console.WriteLine("StudentInfos:");
            InfoOutput.PrintStudentInfos(studentInfos);
            Console.WriteLine("--------------------------------------------------");

            IMapper mapper = MappingTableConfiguration.CreateMappingTable();
            StudentInfoDto[] studentInfoDtos = mapper.Map<StudentInfo[], StudentInfoDto[]>(studentInfos);

            Console.WriteLine("After adjusting the score,");
            Console.WriteLine();
            Console.WriteLine("StudentInfos:");
            InfoOutput.PrintStudentInfos(studentInfos);
            Console.WriteLine("StudentInfoDtos:");
            InfoOutput.PrintStudentInfoDtos(studentInfoDtos);
            Console.WriteLine("--------------------------------------------------");
        }
```

it will output the following in console.

```
Before adjusting the score,

StudentInfos:
The student:'Yazawa Nico' gets 30 score.
The student:'Ayase Eli' gets 60 score.
The student:'Minami Kotori' gets 20 score.
--------------------------------------------------
After adjusting the score,

StudentInfos:
The student:'Yazawa Nico' gets 60 score.
The student:'Ayase Eli' gets 100 score.
The student:'Minami Kotori' gets 40 score.
StudentInfoDtos:
The student:'Yazawa Nico' gets 60 score.
The student:'Ayase Eli' gets 100 score.
The student:'Minami Kotori' gets 40 score.
--------------------------------------------------
```

<img width="856" alt="image" src="https://github.com/user-attachments/assets/49c3b5d7-adac-4df8-adbe-e37022999a42" />

see example 1 for complete code.

Story:

The kindly teacher found the fact that even she multiply score by two for all student (of course, clampping the score between 0 and 100).

But there is a sttudent named 'Minami Kotori' still failed the test (i.e. score < 60 ) as she gets 40 score.

She want to make every studen pass the test. 

How can she do? 

She founds the solution ad takes a note in the next section.

## CH11.2 -- specify actions after mapping (`AfterMap`)
To map data then process them, we can mapping data (using such as `mapper.Map<TSource,TDestination>` instance method call), first, then write code to process data.

However, there is a simple way to do that in recent version of `AutoMapper`. 

To do that, similar to `specify actions before mapping`, just simply chain `.AfterMap` method just after creating the mapping table (such as `AutoMapper.CreateMap<TSource,TDestination>`).

```
        public StudentInfoProfile()
        {
            CreateMap<StudentInfo, StudentInfoDto>()
                /// to make every student passes the test by adjusting the score to 60 for those students who fails test (i.e. score is under 60) 
                .AfterMap((studentInfo, studentInfoDto) =>
                    {
                        int oldScore = studentInfoDto.Score;
                        int newScore = oldScore >= 60 ? oldScore : 60;
                        studentInfoDto.Score = newScore;
                    }
                )
                ;
        }
```

Take preceeding example for example,

Modifies `StudentInfoProfile.cs` by adding `.AfterMap` method call.

`StudentInfoProfile.cs`

```
    public class StudentInfoProfile: Profile
    {
        public StudentInfoProfile()
        {
            CreateMap<StudentInfo, StudentInfoDto>()
                .BeforeMap((studentInfo, studentInfoDto) =>
                    {
                        GeometricAdjustificationHandler geometricAdjustificationHandler = new GeometricAdjustificationHandler(100, 0);
                        studentInfo.Score = geometricAdjustificationHandler.Adjust(studentInfo, 2);
                    }
                )
                /// to make every student passes the test by adjusting the score to 60 for those students who fails test (i.e. score is under 60) 
                .AfterMap((studentInfo, studentInfoDto) =>
                    {
                        int oldScore = studentInfoDto.Score;
                        int newScore = oldScore >= 60 ? oldScore : 60;
                        studentInfoDto.Score = newScore;
                    }
                )
                ;
        }
    }
```

When invoking the following test method:

```
        public static void TestMethod1()
        {
            StudentInfo[] studentInfos = new StudentInfo[]
            {
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Yazawa",
                        lastName = "Nico"
                    },
                    Score = 30
                },
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Score = 60
                },
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Minami",
                        lastName = "Kotori"
                    },
                    Score = 20
                },
            };


            Console.WriteLine("Before adjusting the score,");
            Console.WriteLine();
            Console.WriteLine("StudentInfos:");
            InfoOutput.PrintStudentInfos(studentInfos);
            Console.WriteLine("--------------------------------------------------");

            IMapper mapper = MappingTableConfiguration.CreateMappingTable();
            StudentInfoDto[] studentInfoDtos = mapper.Map<StudentInfo[], StudentInfoDto[]>(studentInfos);

            Console.WriteLine("After adjusting the score,");
            Console.WriteLine();
            Console.WriteLine("StudentInfos:");
            InfoOutput.PrintStudentInfos(studentInfos);
            Console.WriteLine("StudentInfoDtos:");
            InfoOutput.PrintStudentInfoDtos(studentInfoDtos);
            Console.WriteLine("--------------------------------------------------");
        }
```

it will output the following in console.

```
Before adjusting the score,

StudentInfos:
The student:'Yazawa Nico' gets 30 score.
The student:'Ayase Eli' gets 60 score.
The student:'Minami Kotori' gets 20 score.
--------------------------------------------------
After adjusting the score,

StudentInfos:
The student:'Yazawa Nico' gets 60 score.
The student:'Ayase Eli' gets 100 score.
The student:'Minami Kotori' gets 40 score.
StudentInfoDtos:
The student:'Yazawa Nico' gets 60 score.
The student:'Ayase Eli' gets 100 score.
The student:'Minami Kotori' gets 60 score.
--------------------------------------------------
```

<img width="863" alt="image" src="https://github.com/user-attachments/assets/86ffb649-1e1f-4bc1-a957-856263a6135c" />

see example 2 for complete code.

Story:

We can find that each student passes the test (i.e. score >= 60 )

Before `.AfterMap` call, the student named 'Minami Kotori' will fail the test (as she gets `40`) even if adjusting the score at first time.

Thanks to kindly teacher, who wants each student passes the test, but she is lazy for write a repetive loop to adjust the score after mapping.

So, she uses `AutoMapper` API -- `AfterMap`, and put the logic in the lambda expression in `AfterMap` method call.

That's the pro of `AfterMap`. 

However, she is thinking about an question. 

logic in the lambda expression in `AfterMap` method call seems to be a little complicated. 

Are there any ways to make it easuer to read?  

Yes, see next section.

## CH11.3 -- specify actions before mapping or after mapping with template (`Process` method in `IMappingAction` interface)
Logic in the lambda expression in `BeforeMap` or `AfterMap` method call might seem to be a little complicated.

In `AutoMapper, we can extract the logic in the lambda expression in `BeforeMap` or `AfterMap` method call into other place 

and invoke `BeforeMap` or `AfterMap` method call with template.


To create an action and use it as template, follow these steps.

Step 1:

To create an action, create a class that implements `IMappingAction<TSource, TDestination>` interface,

then defines `public void Process(TSource source, TDestination dest, ResolutionContext context)` method.

```
    // `IMappingAction<TSource, TDestination>`
    public class LowestValueAdjustificationHandler: IMappingAction<StudentInfo, StudentInfoDto>
    {
        /// to make every student passes the test by adjusting the score to 60 for those students who fails test (i.e. score is under 60)
        `public void Process(TSource source, TDestination dest, ResolutionContext context)`
        public void Process(StudentInfo studentInfo, StudentInfoDto studentInfoDto, ResolutionContext context)
        {
            // logic
        }
    }
```

Step 2:

place your logic inside `public void Process(TSource source, TDestination dest, ResolutionContext context)` method.

Step 3:

to use the action as template, 

just put it as generic in `BeforeMap` , `AfterMap` instance method.

```
                /// to make every student passes the test by adjusting the score to 60 for those students who fails test (i.e. score is under 60) 
                cfg.AfterMap<LowestValueAdjustificationHandler>()
```

Take proceeding example.

Add `LowestValueAdjustificationHandler.cs`. 

Move the code (about logic) from lamda expression in `AfterMap` method call to `Process` method definition.

```
    public class LowestValueAdjustificationHandler: IMappingAction<StudentInfo, StudentInfoDto>
    {
        /// to make every student passes the test by adjusting the score to 60 for those students who fails test (i.e. score is under 60) 
        public void Process(StudentInfo studentInfo, StudentInfoDto studentInfoDto, ResolutionContext context)
        {
            int oldScore = studentInfoDto.Score;
            int newScore = oldScore >= 60 ? oldScore : 60;
            studentInfoDto.Score = newScore;
        }
    }
```

And next, just simply use `LowestValueAdjustificationHandler` generic type in `AfterMap` method call.

`StudentInfoProfile.cs`

```
    public class StudentInfoProfile: Profile
    {
        public StudentInfoProfile()
        {
            CreateMap<StudentInfo, StudentInfoDto>()
                .BeforeMap((studentInfo, studentInfoDto) =>
                    {
                        GeometricAdjustificationHandler geometricAdjustificationHandler = new GeometricAdjustificationHandler(100, 0);
                        studentInfo.Score = geometricAdjustificationHandler.Adjust(studentInfo, 2);
                    }
                )
                /// to make every student passes the test by adjusting the score to 60 for those students who fails test (i.e. score is under 60) 
                .AfterMap<LowestValueAdjustificationHandler>()
                ;
        }
    }
```

Then running the test method -- `TestMethod1`, same as above

```
        public static void TestMethod1()
        {
            StudentInfo[] studentInfos = new StudentInfo[]
            {
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Yazawa",
                        lastName = "Nico"
                    },
                    Score = 30
                },
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Ayase",
                        lastName = "Eli"
                    },
                    Score = 60
                },
                new StudentInfo()
                {
                    Student = new User()
                    {
                        firstName = "Minami",
                        lastName = "Kotori"
                    },
                    Score = 20
                },
            };


            Console.WriteLine("Before adjusting the score,");
            Console.WriteLine();
            Console.WriteLine("StudentInfos:");
            InfoOutput.PrintStudentInfos(studentInfos);
            Console.WriteLine("--------------------------------------------------");

            IMapper mapper = MappingTableConfiguration.CreateMappingTable();
            StudentInfoDto[] studentInfoDtos = mapper.Map<StudentInfo[], StudentInfoDto[]>(studentInfos);

            Console.WriteLine("After adjusting the score,");
            Console.WriteLine();
            Console.WriteLine("StudentInfos:");
            InfoOutput.PrintStudentInfos(studentInfos);
            Console.WriteLine("StudentInfoDtos:");
            InfoOutput.PrintStudentInfoDtos(studentInfoDtos);
            Console.WriteLine("--------------------------------------------------");
        }
```

it will output following in console.

```
Before adjusting the score,

StudentInfos:
The student:'Yazawa Nico' gets 30 score.
The student:'Ayase Eli' gets 60 score.
The student:'Minami Kotori' gets 20 score.
--------------------------------------------------
After adjusting the score,

StudentInfos:
The student:'Yazawa Nico' gets 60 score.
The student:'Ayase Eli' gets 100 score.
The student:'Minami Kotori' gets 40 score.
StudentInfoDtos:
The student:'Yazawa Nico' gets 60 score.
The student:'Ayase Eli' gets 100 score.
The student:'Minami Kotori' gets 60 score.
--------------------------------------------------
```

<img width="862" alt="image" src="https://github.com/user-attachments/assets/9797cfde-6a30-4b2b-9f4e-ca0ae9d09cb4" />

see example 3 for complete code.

Story:

The kindly teacher finally founds a way to make it easier.

Just implement `AutoMapper.IMappingAction<TSource,TDestination>` and defines the `public void Process(TSource source,TDestination dest,ResolutionContext context)` method.

## CH11.4 -- pros
Without `AutoMapper` API -- `BeforeMap` and `AfterMap`,

one needs to do one of the following.

1. write a piece of code before or after mapping data (such as `mapper.Mapper<TSource,TDestination>` statement), or

2. mix the logic of processing data into actions of mapping table (such as put the logic into `.ForMember` method call, or add extra `.ForMember` method call).

Both ways has fatal cons.

+ In former way, when using such as `mapper.Mapper<TSource,TDestination>` statement, one has to write a piece of code. 

The code snippet about processing data will NOT be fully reused 

However, it can be reusable by extracting the code into method, but there is still one fatal cons.

For most people, one usually forgets to invoke the (extracted) method once one maps data, and thus get unexpected result.  

+ In latter way, mixing the logic (about process data) into other logic (way how to map the table) can make it harder to read and understand.

So, it is better worth to use `AutoMapper` API -- `BeforeMap` and `AfterMap` (if possible).

It has following pros, including:

+ seperate the logic (about prcoessing data), making it more readable, maintenable.
+ avoid to invoke the method (for prcoessing data), making it more maintenable.


## examples
### example 1
#### demo project
see [AutoMapper demo8.7z (version 1.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo8/1.0.0/AutoMapper%20demo8.7z)

### example 2
#### demo project
see [AutoMapper demo8.7z (version 2.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo8/2.0.0/AutoMapper%20demo8.7z)

### example 3
#### demo project
see [AutoMapper demo8.7z (version 3.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo8/3.0.0/AutoMapper%20demo8.7z)
