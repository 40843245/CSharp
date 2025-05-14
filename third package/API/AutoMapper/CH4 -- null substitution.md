# CH4 -- null substitution
## objectives
In this article, you will learn 

+ what is null substitution
+ how to do null substitution

## CH4.1 -- what is null substitution
Null substitution refers to substitute the value iff it is evaluated to null.

## CH4.2 -- how to do null substitution
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

part code of example 1.

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

see example 1 for full example.

## examples
### example 1
#### demo project
See [`AutoMapper demo5.7z (version 1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo5/1.0.0/AutoMapper%20demo5.7z)

## reference
### further reading
+ [`Null Substitution`](https://docs.automapper.org/en/stable/Null-substitution.html)
