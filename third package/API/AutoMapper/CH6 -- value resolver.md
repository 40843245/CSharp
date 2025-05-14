# CH6 -- value resolver
## objectives
In this article, you will learn how to

+ define a value resolver and use it
  
## CH6.1 -- define a value resolver and use it
Step 1:

To customize a value resolver, you need to define a class that implements `IValueResolver<TSource,TDestination,TReturnType>` interface.

By the definition of `IValueResolver<TSource,TDestination,TReturnType>` interface,

you will need to defines a method inside the class.

```
public TReturnType Resolve(TSource source, TDestination destination, TReturnType destMember, ResolutionContext context)`
{
    // your logic
}
```

Step 2:
> [!CAUTION]
> `ResolveUsing` is deprecated in `AutoMapper` package in version 8.0.0.
>
> For later version, use `MapFrom` instead, the syntax of `MapFrom` is equivalent to that of `ResolveUsing`.
>
> For example,
> 
> ```
> cfg.ForMember(userDTO => userDTO.AGE, action => action.MapFrom<AgeResolver>())
> ```

Then one can invoke `ResolveUsing` instance method in the lambda expression about action 

with generic type `IValueResolver<TSource,TDestination,TReturnType>` inside `ForMember` instance method call.

For example,

I defined a class `AgeResolver` which implements `IValueResolver<User, UserDTO, int>` and define the Resolve method.

```
    public class AgeResolver : IValueResolver<User, UserDTO, int>
    {
        #region implement all interfaces
        public int Resolve(User source, UserDTO destination, int destMember, ResolutionContext context)
        {
            DateTime now = DateTime.Now;

            DateTime birthdate = source.birthdate;

            int age = now.Year - birthdate.Year;

            // Check if the birthday has occurred this year
            if (birthdate > now.AddYears(-age))
            {
                age--;
            }

            return age;
        }
        #endregion
    }
```

Then I can use it as follows.

```
                cfg.CreateMap<User, UserDTO>()
                   .ForMember(userDTO => userDTO.AGE, action => action.ResolveUsing<AgeResolver>())
```

see example 1 for complete code.

## examples
### example 1
#### demo project
See [`AutoMapper demo2.7z (version 2.0.0)`](https://github.com/40843245/CSharp-Demo-Project/tree/main/AutoMapper/AutoMapper%20demo2/2.0.0/AutoMapper%20demo2.7z)

## reference
### further reading
+ [`Custom Value Resolvers`](https://docs.automapper.org/en/stable/Custom-value-resolvers.html)
