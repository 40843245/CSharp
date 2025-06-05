# CH3 -- check a method is public etc
## objectives
You will learn how to

+ check a method is public, private, a constructor, virtual (marked with modifier `virtual`), abstract (marked with modifier `abstract`)

+ check a method defined with generic

+ and more...

## CH3.1 -- check a method is public etc
### `IsPublic` property
The name is self-explanatory.

Iff this method is public, then it will return true.

### `IsPrivate` property
The name is self-explanatory.

Iff this method is private, then it will return true.

### `IsConstructor` property
The name is self-explanatory.

Iff this method is a constructor, then it will return true.

> [!NOTE]
> Quick Review:
> 
> What is constructor?
>
> A constructor is a method that is called when instantiating an instance.
>
> It is usually used for initial properties.

An example of constructor,

In `C#`

```
public class Point
{
    public double X {get; set;}
    public double Y {get; set;}
    public Point(double x,double y) 
    // this is a constructor, and also is public.
    {
        X = x;
        Y = y;
    }
}
```

### `IsVirtual` property
The name is self-explanatory.

Iff this method is virtual (i.e. is marked with modifier `virtual`), then it will return true.

> [!NOTE]
> Quick Review:
> 
> What is `virtual` modifier?
>
> A method marked with `virtual` modifier makes it virtual, not in reality.
>
> If a method is NOT marked with `virtual` modifier, 
>
> then this method can NOT be overridden in derived class.
>
> P.S. 
>
> Even a method is marked with `virtual` modifier, 
>
> it is NOT necessary that this method can be overridden in derived class
> 
> For more details, see [`virtual (C# Reference)`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/virtual)

### `IsAbstract` property
The name is self-explanatory.

Iff this method is abstract (i.e. is marked with modifier `abstract`), then it will return true.

> [!NOTE]
> Quick Review:
> 
> What is `abstract` modifier?
>
> A method marked with `abstract` modifier makes it abstract 
>
> so that the method can NOT be **defined**, can ONLY be **declared**.
>
> Additionally, if the derived class is NOT marked with `abstract` modifier
>
> then the derived class MUST implement all methods that are marked with `abstract` modifier.
> 
> For more details, see [`abstract (C# Reference)`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/abstract)

### `IsFinal` property
The name is self-explanatory.

Iff this method is final (i.e. is marked with modifier `sealed`), then it will return true.

> [!NOTE]
> Quick Review:
> 
> What is `sealed` modifier?
>
> A method marked with `sealed` modifier behaves like seal it in the box 
>
> so that the method can NOT be **overriden** in derived class.
>
> For more details, see [`sealed (C# Reference)`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/sealed)

### `IsStatic` property
The name is self-explanatory.

Iff this method is static (i.e. is marked with modifier `static`), then it will return true.

### `IsGenericMethod`
The name is self-explanatory.

Iff this method is a generic method (i.e. is defined with generic type), then it will return true.

For example,

```
public class Point
{
    public double X {get; set;}
    public double Y {get; set;}

    // ... omitted for clarity

    public Point move<T>(T offsetX)
    // this is defined with generic type and also is public.
    {
        return new Point(other.X+ (double)offsetX, other.Y);
    }
}
```

### `IsGenericMethodDefinition` property
Iff the `MethodBase` object represents the definition of a generic method, then it will return true. 

#### Remarks
see [Remarks in `MethodBase.IsGenericMethodDefinition Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.isgenericmethoddefinition?view=net-9.0#remarks)

### `IsSpecialName` property
Iff the method has a special name, then it will return true. 

For more details, see [`MethodBase.IsSpecialName Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.isspecialname?view=net-9.0)

### `IsHideBySig` property
`Sig` stands for `signature`.

`IsHideBySig` property means that this method is hidden by its signature.

Exactly said, iff a derived class hides the member defined in base class

by its signature, it will return true.

see [first example in `MethodBase.IsHideBySig Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.ishidebysig?view=net-9.0#examples) for more understanding.

### `IsFamily` property
Iff access to this method or constructor is exactly described by [Family](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-family),

then it will return true.

### `IsAssembly` property
Iff the visibility of this method or constructor is exactly described by [Assembly](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-assembly)

then it will return true.

### `IsFamilyAndAssembly` property
Iff access to this method or constructor is exactly described by [FamANDAssem](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-famandassem) (which is described by **both** [Family](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-family) **and** [Assembly](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-assembly))

then it will return true.

### `IsFamilyOrAssembly` property
Iff access to this method or constructor is exactly described by [FamORAssem](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-famorassem) (which is described by **either** [Family](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-family) **or** [Assembly](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-assembly), **or both**)

then it will return true.

### `IsSecurityCritical` property
Iff this method or constructor is security-critical or security-safe-critical at the current trust level, 

then it will return true.

### `IsSecuritySafe` property
Iff the method or constructor is security-safe-critical at the current trust level, 

then it will return true.

### `IsSecurityTransparent` property
Iff the method or constructor is security-transparent at the current trust level,

then it will return true.

## examples
### example 1
see demo project

## reference
### API docs
+ [`MethodBase Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase?view=net-9.0)

## further reading
### API docs
About family, assembly,

+ [`Family`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-family)

+ [`Assembly`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-assembly)

+ [`FamANDAssem`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-famandassem)

+ [`FamORAssem`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodattributes?view=net-9.0#system-reflection-methodattributes-famorassem)