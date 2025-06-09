# CH3 -- get the description of specific property
## objectives
You will learn how to

+ get the description of specific property

## CH3.1 -- get the description of specific property

Step 1:

Invoking `TypeDescriptor.GetProperties` static method

```
// .. obj is `Object` type.

TypeDescriptor.GetProperties(obj)
```

Step 2:

To get the value with key property's name of resultant dictionary in Step 1.

```
// .. obj is `Object` type.

TypeDescriptor.GetProperties(obj)[propertyName]
```

where

`propertyName` MUST be a string that represents the property's name.

For example,

If I want to get a property with name `Owner`.

I can hardcode `"Owner"` or use `nameof(new SociaMediaChannel().Owner)` (which is more flexible)

```
    public class SociaMediaChannel
    {
        [Description("PlayLists that category all videos")]
        public PlayLists PlayLists { get; init; }

        [Description("The info about owner of this channel in Social Media App")]
        public User Owner { get; init; }
    }
```

> [!CAUTION]
> If no such property found with given property name (using `TypeDescriptor.GetProperties(obj)[propertyName]`),
>
> it will throw `System.NullReferenceException` exception at runtime.

Step 3:

Get their attributes by accessing `Attributes` property.

It will return `AttributeCollection`.

```
AttributeCollection attributeCollection = TypeDescriptor.GetProperties(obj)[propertyName].Attributes;
```

Step 4:

Get the description by index of attributes (result in Step 3) 

and then cast it to `DescriptionAttribute` type. 

```
(DescriptionAttribute)attributeCollection[typeof(DescriptionAttribute)];
```

Full code snippets:

```
public static DescriptionAttribute GetDescriptionAttribute(
    this object obj,
    string propertyName
)
{
        AttributeCollection attributeCollection = TypeDescriptor.GetProperties(obj)[propertyName].Attributes;
        return (DescriptionAttribute)attributeCollection[typeof(DescriptionAttribute);
}
```

## examples
### example 1
see example 1 in [CH2 -- describe a property with `DescriptionAttribute` attribute inside annotation](CH2%20--%20describe%20a%20property%20with%20`DescriptionAttribute`%20attribute%20inside%20annotation.md)

## reference
### API reference
+ [`DescriptionAttribute Class`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.descriptionattribute?view=net-8.0)

+ [`TypeDescriptor.GetProperties Method`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.typedescriptor.getproperties?view=net-8.0)

+ [`AttributeCollection Class`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.attributecollection?view=net-9.0)