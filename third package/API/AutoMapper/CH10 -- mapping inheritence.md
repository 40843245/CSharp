# CH10 -- mapping inheritence
## objectives
In this article, you will learn how to

+ configure inheritance from the base class
+ specify inheritance in derived classes
+ redirect a base map to an existing derived map

Also, you will know Inheritance Mapping Priorities (i.e. priorities of inheritence mapping)

## CH10.1 -- configure inheritance from the base class
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

See example 1 for full code.

## CH10.2 -- specify inheritance in derived classes
Instead of configuring inheritance from the base class, you can specify inheritance from the derived classes.

For example:

`Order.cs`

```
public class Order
{
    public int OrderId {get;set;} 
}
public class OnlineOrder : Order { }
public class MailOrder : Order { }
```

`OrderDto.cs`

```
public class OrderDto 
{
    public int Id { get; set; }
}
public class OnlineOrderDto : OrderDto { }
public class MailOrderDto : OrderDto { }
```

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

See example 2 for full code.

For more details, see [Specifying inheritance in derived classes](https://docs.automapper.org/en/stable/Mapping-inheritance.html#runtime-polymorphism) 

## CH10.3 -- redirect a base map to an existing derived map
For simple cases, you can use As to redirect a base map to an existing derived map

```
    cfg.CreateMap<Order, OnlineOrderDto>();
    cfg.CreateMap<Order, OrderDto>().As<OnlineOrderDto>();
    
    mapper.Map<OrderDto>(new Order()).ShouldBeOfType<OnlineOrderDto>();
```

## CH10.4 -- Inheritance Mapping Priorities
priorities of inheritance mapping follows the order. 

1. Explicit Mapping (using `MapFrom` instance method)
2. Inherited Explicit Mapping
3. Ignore Property Mapping (using `Ignore` instance method)
4. Convention Mapping (Properties that are matched via convention)

To demonstrate this, lets modify our classes from example 2.

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

See example 3 for full code.

## examples
### example 1
#### demo project
See [`AutoMapper demo5.7z (version (1.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/third%20packages/AutoMapper/AutoMapper%20demo5/1.0.0/AutoMapper%20demo5.7z)

### example 2
#### demo project
See [`AutoMapper demo5.7z (version (2.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/third%20packages/AutoMapper/AutoMapper%20demo5/2.0.0/AutoMapper%20demo5.7z)

### example 3
#### demo project
See [`AutoMapper demo5.7z (version (3.0.0)`](https://github.com/40843245/CSharp-Demo-Project/blob/main/third%20packages/AutoMapper/AutoMapper%20demo5/3.0.0/AutoMapper%20demo5.7z)

## reference
### further reading
+ [Runtime polymorphism](https://docs.automapper.org/en/stable/Mapping-inheritance.html#runtime-polymorphism)
+ [Specifying inheritance in derived classes](https://docs.automapper.org/en/stable/Mapping-inheritance.html#runtime-polymorphism) 
