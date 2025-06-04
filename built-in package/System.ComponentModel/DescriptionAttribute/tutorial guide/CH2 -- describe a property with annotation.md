# CH2 -- describe a property with annotation
## objectives
You will learn how to

+ describe a property with annotation

## CH2.1 -- describe a property with annotation
### `DescriptionAttribute` annotation 

In this example, 

it will describe two properties 

+ `PlayLists` with description `PlayLists that category all videos`
+ `Owner` with description `The info about owner of this channel in Social Media App`

```
using System.ComponentModel;

// ...
public class SociaMediaChannel
{
    [Description("PlayLists that category all videos")]
    public PlayLists PlayLists { get; init; }

    [Description("The info about owner of this channel in Social Media App")]
    public User Owner { get; init; }
}
```

> [!NOTE]
> `DescriptionAttribute` can be shortened as `Description`.

## examples
### example 1
#### main code
Invoking following method
## reference
### API docs
+ [`DescriptionAttribute Class`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.descriptionattribute?view=net-8.0)