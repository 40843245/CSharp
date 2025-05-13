# CH11 -- actions before mapping
## objectives
In this article, you will learn how to

+ specify actions before mapping in mapper configuration

## CH11.1 -- specify actions before mapping in mapper configuration
To map from preprocessed data, we can preprocess data before mapping then mapping them (using such as `mapper.Map<TSource,TDestination>` instance method call).

However, it's so troublesome.

In recent version of `AutoMapper`, we can simply chain `.BeforeMap` method followed by `AutoMapper.CreateMap<TSource,TDestination>`.

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

## examples
### example 1
#### demo project
see [AutoMapper demo8.7z (version 1.0.0)](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo8/1.0.0/AutoMapper%20demo8.7z)
