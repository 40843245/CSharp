# CH8 -- read rest of file content asynchronously from stream reader
## objectives
You will learn how to

+ read rest of file content asynchronously from stream reader

## CH8.1 -- read rest of file content asynchronously from stream reader
### `ReadToEndAsync` instance method
It will **asynchronously** read rest of file content from stream reader and 

advance it to the end of stream 

then return `Task<string>` with result -- 

the rest of file content from stream reader.

If it points at the end of the stream,

then it will return `Task<string>` with result -- empty string.

## examples
### example 1
#### main code
Assume `Words.txt` contains these text.

```
Hello
World
A
File
Contain
A
List
Of
Words
```

Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + read rest of file content asynchronously from a file using `System.IO.StreamReader` and `TaskHelper.RunWithTimeout`.
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            Task<string>? readLineTask1 = null;
            Task<string>? readLineTask2 = null;

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                readLineTask1 = TaskHelper.RunWithTimeout<string>(
                    () => streamReader.ReadToEndAsync() , 
                    TimeSpan.FromSeconds(5)
                ); // Wait for the task to complete
                string fileContent = readLineTask1.Result ?? string.Empty;
                string fileContentWithReplacement = fileContent.Replace("\n" , "\\n").Replace("\r" , "\\r");
                Console.WriteLine("Before reading rest of file content through invoking `ReadToEndAsync` method,");
                Console.WriteLine("file content:\n{0}" , fileContentWithReplacement);
                Console.WriteLine("Is file content either null or empty? {0}" , string.IsNullOrEmpty(fileContentWithReplacement));

                readLineTask2 = TaskHelper.RunWithTimeout<string>(
                    () => streamReader.ReadToEndAsync() ,
                    TimeSpan.FromSeconds(5)
                ); // Wait for the task to complete

                string fileContentAfterReadingToEnd = readLineTask2.Result ?? string.Empty;
                string fileContentAfterReadingToEndWithReplacement = fileContentAfterReadingToEnd.Replace("\n" , "\\n").Replace("\r" , "\\r");
                Console.WriteLine("After reading rest of file content through invoking `ReadToEndAsync` method,");
                Console.WriteLine("file content:\n{0}" , fileContentAfterReadingToEndWithReplacement);
                Console.WriteLine("Is file content either null or empty? {0}" , string.IsNullOrEmpty(fileContentAfterReadingToEndWithReplacement));
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod8 method call,
Before reading rest of file content through invoking `ReadToEndAsync` method,
file content:
Hello\r\nWorld\r\nA\r\nFile\r\nContain\r\nA\r\nList\r\nOf\r\nWords
Is file content either null or empty? False
After reading rest of file content through invoking `ReadToEndAsync` method,
file content:

Is file content either null or empty? True
End of TestMethod8 method call,
```

## reference
### API docs
+ [`StreamReader.ReadToEndAsync Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readtoendasync?view=net-8.0)