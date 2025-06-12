# CH4 -- clear buffer data of stream writer
## objectives
You will learn how to

+ clear buffer data of stream writer

## CH4.1 -- clear buffer data of stream writer
### `Flush` instance method
writes all buffered data of stream writer to the underlying device, 

then clear all buffered data.

### `FlushAsync` instance method
**asynchronously** writes all buffered data of stream writer to the underlying device, 

then **asynchronously** clear all buffered data.

### Remarks
> [!IMPORTANT]
> If you set `AutoFlush` property to true, then 
>
> `Flush` or `FlushAsync` instance method call is NOT needed. 
> 
> After each operation about `StreamWriter` (i.e. each method call of `StreamWriter`), it will auto flush the buffer.


> [!CAUTION]
> If you set `AutoFlush` property to true 
>
> and the next operation about `StreamWriter` starts to execute
> 
> when the current operation about `StreamWriter` does NOT finished 
> 
> (the scenario usually occurs when 
>
> performing an **asynchronously** operation about `StreamWriter` but without waiting, 
>
> then it probably throw an exception `System.IO.InvalidOperationException` at runtime 
> 
> to prevent data race condition of the stream writer.

For example,

The following code snippet will probably throw an exception at runtime,

since, when `streamWriter.WriteAsync('\n');` statement is executed,

`streamWriter.WriteLineAsync()` statement execution does NOT be done.

```
streamWriter.AutoFlush = true;
streamWriter.WriteLineAsync();
streamWriter.WriteAsync('\n'); // <-- probably throw an exception at runtime.
```

Corrected code snippet:

Use `await` to ensure the the followed statement of `streamWriter.WriteLineAsync()` statement is executed only if 

`streamWriter.WriteLineAsync()` statement execution is done.

```
streamWriter.AutoFlush = true;
await streamWriter.WriteLineAsync();
await streamWriter.WriteAsync('\n');
```

## examples
### example 1
see example 1 in CH3.

### example 2
see example 1 in CH3.

## reference
### API docs
+ [`StreamWriter.AutoFlush Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.autoflush?view=net-8.0)

+ [`StreamWriter.Flush Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.flush?view=net-8.0)

+ [`StreamWriter.FlushAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.flushasync?view=net-8.0)
