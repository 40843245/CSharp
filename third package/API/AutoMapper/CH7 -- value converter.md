# CH7 -- value converter
## objectives
In this article, you will learn how to

+ create a value converter and use it

## CH7.1 -- create a value converter and use it

Step 1:

defines a class that implements `IValueConverter<in TSourceMember, out TDestinationMember>` interface.

and then defines `public TDestinationMember Convert(TSourceMember source, ResolutionContext context)` method.

```
  public class CurrencyFormatter: IValueConverter<decimal, string>
    {
        public string Convert(decimal source, ResolutionContext context)
        {      
            // logic
        }
    }
```

Step 2:

To use it, invoke `ConvertUsing<TValueConverter, TSourceMember>()` 

where 

`TValueConverter` : `IValueConverter<TSourceMember, TMember>`

```
          CreateMap<InnerSource, InnerDest>()
                       .ForMember(
                            innerDest => innerDest.DisplayedTextForOtherValue,
                            action =>
                            {
                                action.ConvertUsing<CurrencyFormatter, decimal>();
                                action.MapFrom(innerSource => innerSource.OtherValue);
                            }
                        )
                       ;
```

For example,

Defines beans and DTOs.

`InnerSource.cs`

```
    public class InnerSource
    {
        public decimal OtherValue { get; set; }
    }
```

`InnerDest.cs`

```
    public class InnerDest
    {
        public decimal OtherValue { get; set; }
        public string DisplayedTextForOtherValue { get; set; }

    }
```

Define a value converter

`CurrencyFormatter.cs`

```
using AutoMapper;
using System.Globalization;

    public class CurrencyFormatter: IValueConverter<decimal, string>
    {
        public string Convert(decimal source, ResolutionContext context)
        {      
            // using  System.Globalization;
            NumberFormatInfo numberFormatInfo = new NumberFormatInfo();
         
            numberFormatInfo.CurrencyDecimalSeparator = ",";
            numberFormatInfo.CurrencyDecimalDigits = 3;

            decimal val = source;
            string displayText = val.ToString(numberFormatInfo);
            return displayText;
        }
    }
```

Define a profile.

`InnerProfile.cs`

```
    public class InnerProfile : Profile
    {
        public InnerProfile() 
        {
            CreateMap<InnerSource, InnerDest>()
                        .ForMember(
                            innerDest => innerDest.OtherValue,
                            action => action.MapFrom(innerSource => innerSource.OtherValue)
                        )
                       .ForMember(
                            innerDest => innerDest.DisplayedTextForOtherValue,
                            action =>
                            {
                                action.ConvertUsing<CurrencyFormatter, decimal>();
                                action.MapFrom(innerSource => innerSource.OtherValue);
                            }
                        )
                       ;
        }
    }
```

Configure mapper configuration.

`InnerAndOuterMappers.cs`

```
    public class InnerAndOuterMappers
    {

        public static IMapper CreateMappingTable()
        {

           var configuration = new MapperConfiguration(cfg =>
           {
              cfg.AddProfile<InnerProfile>();
           });
           IMapper mapper = configuration.CreateMapper();
           return mapper;
        }
    }
```

Then invoking the test method 

```
        public static void TestClass7()
        {
            var innerSourceArray = new InnerSource[]
            {
                new InnerSource
                {
                    OtherValue = 5000000
                },
                new InnerSource
                {
                    OtherValue = 2000
                },
                new InnerSource
                {
                    OtherValue = 10
                }
            };

            IMapper mapper = InnerAndOuterMappers.CreateMappingTable();
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
innerDest.OtherValue:5000000
innerDest.DisplayedTextForOtherValue:5,000,000

innerDest.OtherValue:2000
innerDest.DisplayedTextForOtherValue:2,000

innerDest.OtherValue:10
innerDest.DisplayedTextForOtherValue:10

--------------------------------------------
```

## reference
### further reading
+ [`Value Converters`](https://docs.automapper.org/en/stable/Value-converters.html)
