# CH2 -- read one line asynchronously from stream reader
## objectives
You will learn how to

+ read one line asynchronously from stream reader

## CH2.1 -- read one line asynchronously from stream reader
### `ReadLineAsync` instance method
will **asynchronously** read a line from position 

(that current pointer points to) to the end of the line and

advance the pointer of the stream to beginning of next line.

then returns the `Task<string>` instance containing the line that 

is read.

If it reaches the end (meaning that there are more available lines to be read), 

then it will return `Task<string>` instance containing `null`.

The following code snippet prints the file content line-by-line 

(but **asynchronously** read one line).

```
string? line = string.Empty;
Task<string?>? readToEndTask = null;

using(StreamReader streamReader = new StreamReader(filePath))
{
    readToEndTask = TaskHelper.RunWithTimeout<string?>(
        () => streamReader.ReadLineAsync() , 
        TimeSpan.FromSeconds(5)
    ); // Wait for the task to complete
    while(readToEndTask != null)
    {
        line = readToEndTask.Result;
        if(line == null)
        {
            break; // No more lines to read
        }
        Console.WriteLine(line);
        readToEndTask = TaskHelper.RunWithTimeout<string?>(
            () => streamReader.ReadLineAsync() ,
            TimeSpan.FromSeconds(5)
        ); // Wait for the task to complete   
    }                
}
```

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + read file content line-by-line asynchronously from a file using `System.IO.StreamReader` and `TaskHelper.RunWithTimeout`.
        /// </summary>
        public static void TestMethod10()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            string? line = string.Empty;
            Task<string?>? readToEndTask = null;

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                readToEndTask = TaskHelper.RunWithTimeout<string?>(
                    () => streamReader.ReadLineAsync() , 
                    TimeSpan.FromSeconds(5)
                ); // Wait for the task to complete
                while(readToEndTask != null)
                {
                    line = readToEndTask.Result;
                    if(line == null)
                    {
                        break; // No more lines to read
                    }
                    Console.WriteLine(line);
                    readToEndTask = TaskHelper.RunWithTimeout<string?>(
                        () => streamReader.ReadLineAsync() ,
                        TimeSpan.FromSeconds(5)
                    ); // Wait for the task to complete   
                }                
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

might output following

```
In TestMethod10 method call,
Hello
World
A
File
Contain
A
List
Of
Words
End of TestMethod10 method call,
```
## reference
### API docs
+ [`StreamReader Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader?view=net-8.0)

+ [`StreamReader.ReadLineAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readlineasync?view=net-8.0)