# CH16 -- get standard input stream
## objectives
You will learn how to

+ get standard input stream

## CH16.1 -- get standard input stream
### `Console.OpenStandardInput` static method

```
Stream standardInputStream = Console.OpenStandardInput();
```

will acquire the standard input stream.

```
TextReader standardedInputStream = new StreamReader(Console.OpenStandardInput());
Console.SetIn(standardedInputStream);
```

will set the input stream as standard input stream.

## examples
### example 1
see example 2 in CH15.
