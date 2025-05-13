# CH5 -- mapping a member with condition or pre-condition
## objectives
In this article, you will learn how to

+ mapping a member with condition
+ mapping a member with pre-condition

## CH5.1 -- mapping a member with condition
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

Run code snippets in example 1.

## examples
### example 1
#### demo project
See [`AutoMapper demo2.7z (version 3.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/3.0.0/AutoMapper%20demo2.7z)

## reference
### examples
examples modified from examples in [Conditional Mapping](https://docs.automapper.org/en/stable/Conditional-mapping.html#conditional-mapping)

## further reading 
[Conditional Mapping](https://docs.automapper.org/en/stable/Conditional-mapping.html#conditional-mapping)
