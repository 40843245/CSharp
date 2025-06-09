# Func
## basic info
Namespace:
System

Assembly:
System.Runtime.dll

## definition
```
public delegate TResult Func<in T,out TResult>(T arg);
```

## examples
### example 1

```
Func<int, int, int> randInt = (n1, n2) => new Random().Next(n1, n2); 
```

where 

first type of generic in `Func<int, int, int>` is `int` indicates that return type should be `int`.

second type of generic in `Func<int, int, int>` is `int` indicates that type first argument should be `int`.

third type of generic in `Func<int, int, int>` is `int` indicates that type second argument should be `int`.
## reference
[Func<T,TResult> Delegate](https://learn.microsoft.com/en-us/dotnet/api/system.func-2?view=net-9.0)

[What is Func, how and when is it used](https://stackoverflow.com/questions/3624731/what-is-func-how-and-when-is-it-used)
