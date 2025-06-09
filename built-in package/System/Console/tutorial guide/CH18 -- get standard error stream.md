# CH18 -- get standard error stream
## objectives
You will learn how to

+ get standard error stream

## CH18.1 -- get standard error stream
### `Console.OpenStandardError` static method

```
Stream standardErrorStream = Console.OpenStandardError()
```

will acquire the standard error stream.

```
TextWriter standardErrorStream = new StreamWriter(Console.OpenStandardError())
{
    AutoFlush = true
};
Console.SetError(standardErrorStream);
```

will set the error stream as standard error stream and then
 
instanitate `TextWriter` instance with initialization of `AutoFlush` getter-setter property as true.

## examples
### example 1
see example 1 in CH13.
