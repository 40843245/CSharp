# CH9 -- attribute mapping
## objectives
In this article, you will learn 

+ intro and pro of auto attribute mapping
+ how to auto mapping attribute

## CH9.1 -- intro and pro of auto attribute mapping
When auto mapping attribute from class `TSource` to `TDestination` with decoration `[AutoMap(typeof(TDestination))]`,

one DOESN'T need to explicity map members from class `TSource` to `TDestination (using `.ForMember` instance method) for those members have same name.

For those members with same name, `AutoMap` tells compiler to automatically map from to the source members to the desintaion members.

pro of auto attribute mapping including:

+ reduce code redudancy
+ easier to read
+ easier to maintain
+ etc

To make the reader have more understanding about its pro, I'll take an example and compare the code snippets with auto attribute mapping and without it.


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

+ Without `AutoMap`, or say if the `destination class` -- `OrderDto` class is NOT marked as `AutoMap` with `[AutoMap(typeof(Order))]` decoration, as following defintion.

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

+ With `AutoMap`, or say if the `destination class` -- `OrderDto` class is marked as `AutoMap` with `[AutoMap(typeof(Order))]` decoration, as following defintion.

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


## CH9.2 -- how to auto mapping attribute
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

see example 1 for full code.

## examples
### example 1
#### demo project
See [`AutoMapper demo6.7z (version (1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/AutoMapper/AutoMapper%20demo6/1.0.0/AutoMapper%20demo6.7z)
