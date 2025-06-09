# CH7 -- get attributes of a method implementation
## objectives
You will learn how to

+ get attributes of a method implementation

## CH7.1 -- get attributes of a method implementation
### `MethodImplementationFlags` property
gets [MethodImplAttributes](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodimplattributes?view=net-9.0) flags that specify attributes of a method implementation.

### `GetMethodImplementationFlags` instance method
Do same behavior as `MethodImplementationFlags` property.

However, due to different syntax, the use case might be different.

### `MethodImplementationFlags` property v.s. `GetMethodImplementationFlags` instance method

![syntax of MethodBase.MethodImplementationFlags Property](syntax%20of%20MethodBase.MethodImplementationFlags%20Property.png)

![syntax of MethodBase.GetMethodImplementationFlags instance Method.png](syntax%20of%20MethodBase.GetMethodImplementationFlags%20instance%20Method.png)

| | `MethodImplementationFlags` property | `GetMethodImplementationFlags` instance method |
| :-- | :-- | :-- |
| marked with modifiers | `virtual` | `abstract` |
| need to override in derived class | NOT necessary | always needs to if the derived class is NOT marked as `abstract` |

## reference
### API docs
+ [`MethodBase.MethodImplementationFlags Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.methodimplementationflags?view=net-9.0)

+ [`MethodBase.GetMethodImplementationFlags Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.getmethodimplementationflags?view=net-9.0)

+ [`MethodImplAttributes Enum`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodimplattributes?view=net-9.0)