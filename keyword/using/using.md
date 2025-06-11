# using
The `using` keyword can be used as import statements to 

import namespace and class.

In newer version of `C#`, it also can be used in `using` block that

can dispose the instance defined in `using` block after the execution of `using` block.

## import statements
### import namespace
In example 1, we define 

+ a static class `FilePathHelper`
+ a non-static class `FilePathHandler`

in `Example.Helper.FilePath` namespace.

```
namespace Example.Helper.FilePath
{
    public static class FilePathHelper
    {
        public static string GetRootDirectory()
        {
            // ... logic here
        }
    }
}
```

```
namespace Example.Helper.FilePath
{
    public class FilePathHandler
    {
        public Path GetRootDirectoryPath()
        {
            // ... logic here
        }
    }
}
```

then we can import `Example.Helper.FilePath` namespace.

```
using Example.Helper.FilePath;
```

after that, we can use static class `FilePathHelper`

```
FilePath.FilePathHelper.GetRootDirectory();
```

also we can use non-static class `GetRootDirectoryPath`

```
FileHandler.GetRootDirectoryPath();
```

### import static class
With above example,

we can import static class `FilePathHelper`

```
using static Example.Helper.FilePath.FilePathHelper;
```

after that, we can use static class `FilePathHelper`

```
FilePathHelper.GetRootDirectory();
```

### alias
Take example 1 as example.

We can give alias with `=` operator.

```
using static Example.Helper.FilePath.FilePathHelper = PathHelper;
```

after that, we can use static class `FilePathHelper`

```
PathHelper.GetRootDirectory(); 
```

> [!CAUTION]
> DON'T invoke `FilePathHelper.GetRootDirectory()` since it was dubbed as `PathHelper`.
>
> Otherwise, you will get compile-time error.

## `using` block
In newer version of `C#`, it supports `using` block.

It can automatically dispose the instance 

that is defined in `using` block 

after finishing executing the `using` block.

```
using(StreamReader streamReader = new StreamReader(filePath))
{
    // ... operation

    // it will auto dispose.
}
```

> [!CAUTION]
> The instance that is defined in `using` block MUST implement
>
> `IDisposable` interface. 
>
> Otherwise, it will throw a compile-time error.