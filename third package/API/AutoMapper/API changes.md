# AutoMapper
## API changes
> [!CAUTION]
> This article is last edited at May 12, 2025 which release of `AutoMapper` package is in `14.0.0` currently.
>
> Thus, for `AutoMapper` package in version `14.0.0` (or above), I will NOT discussed it.
>
> Additionally, I did NEVER use `AutoMapper` package in older version.
>
> Thus, for `AutoMapper` package in version `4.2.0` (or earlier), I will NOT discussed it.
>
> You can find the info on [API changes page on offical website](https://docs.automapper.org/en/stable/API-Changes.html), or ask it with the AI model (such as Microsoft Copilot). 

+ About adding existing mapping tables inside map configuration.
  
| method name of</br>a class or an interface | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| `AutoMapper.IMapperConfigurationExpression.AddMaps` | `9.0.0` | NOT obsolete | Yes, fully encouraged. |
| `AutoMapper.IMapperConfigurationExpression.AddProfile` | `5.0.0` | NOT obsolete | No, fully discouraged. |

+ About how to create a mapping table by defining a (Profile) class that implements to `Profile` abstract class.

| technique | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| <ol><li>implements `Profile` abstract class.</li><li>then creates a mapping table inside constructor.</li></ol>| `5.0.0` | NOT obsolete | Yes, fully encouraged. |
| <ol><li>implements `Profile` abstract class.</li><li>then creates a mapping table inside overridden `Configure` method</br>(`protected override void Configure()`).</li></ol>| `1.0.0` | `5.0.0` |  **HAS been obsoleted** |

+ About no need to explicitly map the members those have same name (type attribute -- `AutoMapAttribute`)

| annotation | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| `[AutoMapper.AutoMap]` | `8.0.0` | NOT obsolete | Yes, fully encouraged. |

+ About value resolver.

| method name of</br>a class or an interface | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| `AutoMapper.IMapperConfigurationExpression.ResolveUsing<TResolver>()`,</br>its overloads, and</br>`AutoMapper.IMapperConfigurationExpression.MapFrom(Expression<Func<TSource, TDestination>> mapExpression)` | `1.0.0` | `8.0.0` | **HAS been obsoleted** |
| `AutoMapper.IMapperConfigurationExpression.MapFrom` (three overloads,</br>`MapFrom(Expression<Func<TSource, TDestination>> mappingExpression)`,</br>`MapFrom<TResult>(Func<TSource, TDestination, TResult> mappingFunction)`, and</br>`MapFrom<TResult>(Func<TSource, TDestination, TMember, TResult> mappingFunction)` | `8.0.0` | NOT obsolete | Yes, fully encouraged. |

For more details, see [ResolveUsing](https://docs.automapper.org/en/stable/8.0-Upgrade-Guide.html#resolveusing)
