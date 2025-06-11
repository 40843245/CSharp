# CH2 -- read one line from stream reader
## objectives
You will learn how to

+ read one line from stream reader

## CH2.1 -- read one line from stream reader
### `ReadLine` instance method

It will return the line of current position and move it to next line.

If it reaches the end (meaning that there are more available lines to be read), 

then it will return null.

The following code snippet prints the file content line-by-line.

```
    using(StreamReader streamReader = new StreamReader(filePath))
    {
                string? line = null;
                while ((line = streamReader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
    }
```

## examples
### example 1
see example 1 in CH1.

## reference
### API docs
+ [`StreamReader Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader?view=net-8.0)

+ [`StreamReader.ReadLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readline?view=net-8.0)