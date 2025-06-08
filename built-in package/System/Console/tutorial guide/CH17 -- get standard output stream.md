# CH17 -- get standard output stream
## objectives
You will learn how to

+ get standard output stream

## CH17.1 -- get standard output stream
### `Console.OpenStandardOutput` static method

```
Stream standardOutputStream = Console.OpenStandardOutput()
```

will acquire the standard output stream.

```
TextWriter standardedOutputStream = new StreamWriter(Console.OpenStandardOutput())
{
    AutoFlush = true
};
Console.SetOut(standardedOutputStream);
```

will set the output stream as standard output stream and then
 
instanitate `TextWriter` instance with initialization of `AutoFlush` getter-setter property as true.

## examples
### example 1
see example 2 in CH15.
