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
| `AutoMapper.IMapperConfigurationExpression.AddMaps(string assemblyName)`, and</br>its overloads. | `9.0.0` | NOT obsolete | Yes, fully encouraged. |
| `AutoMapper.IMapperConfigurationExpression.AddProfile<TProfile>()`, and</br>its overloads. | `5.0.0` | NOT obsolete | No, fully discouraged. |

+ About how to create a mapping table by defining a (Profile) class that implements to `Profile` abstract class.

| technique | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| <ol><li>implements `Profile` abstract class.</li><li>then creates a mapping table inside constructor.</li></ol>| `5.0.0` | NOT obsolete | Yes, fully encouraged. |
| <ol><li>implements `Profile` abstract class.</li><li>then creates a mapping table inside overridden `Configure` method</br>(`protected override void Configure()`).</li></ol>| `1.0.0` | `5.0.0` |  **HAS been obsoleted** |

+ About creating a mapping table with static API or NOT.

| technique | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| creating a mapping table with static API | `1.0.0` | `9.0.0` | **HAS been obsoleted** |
| creating a mapping table with instance-based API | `5.0.0` | NOT obsolete | Yes, fully encouraged. |

For more details, see [`The static API was removed`](https://docs.automapper.org/en/stable/9.0-Upgrade-Guide.html)

+ About value resolver.

| method name of</br>a class or an interface | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| `AutoMapper.IMapperConfigurationExpression.ResolveUsing<TResolver>()`, and</br>its overloads, and</br>`AutoMapper.IMapperConfigurationExpression.MapFrom(Expression<Func<TSource, TDestination>> mapExpression)` | `1.0.0` | `8.0.0` | **HAS been obsoleted** |
| `AutoMapper.IMapperConfigurationExpression.MapFrom` (three overloads,</br>`MapFrom(Expression<Func<TSource, TDestination>> mappingExpression)`,</br>`MapFrom<TResult>(Func<TSource, TDestination, TResult> mappingFunction)`, and</br>`MapFrom<TResult>(Func<TSource, TDestination, TMember, TResult> mappingFunction)` | `8.0.0` | NOT obsolete | Yes, fully encouraged. |

For more details, see [ResolveUsing](https://docs.automapper.org/en/stable/8.0-Upgrade-Guide.html#resolveusing)

For the topics NOT dived into, see [Value resolvers](https://docs.automapper.org/en/stable/5.0-Upgrade-Guide.html#value-resolvers)

+ About type converter.

| method name of</br>a class or an interface | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| `AutoMapper.IMapperConfigurationExpression.ConvertUsing(Func<TSource, TDestination> mappingFunction)`, and</br>`AutoMapper.IMapperConfigurationExpression.ProjectUsing(Expression<Func<TSource, TDestination>> mappingExpression)`, and</br>`AutoMapper.IMapperConfigurationExpression.MapFrom(Expression<Func<TSource, TDestination>> mapExpression)` | `1.0.0` | `8.0.0` | **HAS been obsoleted** |
| `AutoMapper.IMapperConfigurationExpression.ConvertUsing(Expression<Func<TSource, TDestination>> mappingExpression)` | `8.0.0` | NOT obsolete | Yes, fully encouraged. |

For more details, see [ProjectUsing](https://docs.automapper.org/en/stable/8.0-Upgrade-Guide.html#projectusing)

For the topics NOT dived into, see [Type converters](https://docs.automapper.org/en/stable/5.0-Upgrade-Guide.html#type-converters)

+ About no need to explicitly map the members those have same name (type attribute -- `AutoMapAttribute`)

| annotation | first released</br>(in which version) | obsolete</br>(in which version) | encouraged by developer |
| :--- | :--- | :--- | :-- |
| `[AutoMapper.AutoMap]` | `8.0.0` | NOT obsolete | Yes, fully encouraged. |
