# CH5 -- read rest of file content from stream reader
## objectives
You will learn how to

+ read rest of file content from stream reader

## CH5.1 -- read rest of file content from stream reader
### `ReadToEnd` instance method
It will read rest of file content from stream reader and 

advance it to the end of stream 

then return the rest of file content from stream reader

If it points at the end of the stream,

then it will return empty string.

## examples
### example 1
#### main code
The following example illustrates how to 

+ read rest characters of file content from stream reader.

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
        /// + read all characters from a file using `System.IO.StreamReader`.
        /// </summary>
        public static void TestMethod7()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);
            using(StreamReader streamReader = new StreamReader(filePath))
            {
                string fileContent = streamReader.ReadToEnd();
                string fileContentWithReplacement = fileContent.Replace("\n" , "\\n").Replace("\r" , "\\r");
                Console.WriteLine("Before reading rest of file content through invoking `ReadToEnd` method,");
                Console.WriteLine("file content:\n{0}" , fileContentWithReplacement);
                Console.WriteLine("Is file content either null or empty? {0}" , string.IsNullOrEmpty(fileContentWithReplacement));

                string fileContentAfterReadingToEnd = streamReader.ReadToEnd();
                string fileContentAfterReadingToEndWithReplacement = fileContentAfterReadingToEnd.Replace("\n" , "\\n").Replace("\r" , "\\r");
                Console.WriteLine("After reading rest of file content through invoking `ReadToEnd` method,");
                Console.WriteLine("file content:\n{0}" , fileContentAfterReadingToEndWithReplacement);
                Console.WriteLine("Is file content either null or empty? {0}" , string.IsNullOrEmpty(fileContentAfterReadingToEndWithReplacement));
                
                int counter = 0;
                int nextCharCode = -1;
                while((nextCharCode = streamReader.Read()) > -1)
                {
                    char nextChar = (char)nextCharCode;
                    string nextString = nextChar.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r");
                    Console.WriteLine("next char:`{0}` next char code:`{1}`" , nextString , nextCharCode);
                    counter++;
                }
                Console.WriteLine("After invoking `ReadToEnd` method, total number of characters read: {0}" , counter);
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod7 method call,
Before reading rest of file content through invoking `ReadToEnd` method,
file content:
Hello\r\nWorld\r\nA\r\nFile\r\nContain\r\nA\r\nList\r\nOf\r\nWords
Is file content either null or empty? False
After reading rest of file content through invoking `ReadToEnd` method,
file content:

Is file content either null or empty? True
After invoking `ReadToEnd` method, total number of characters read: 0
End of TestMethod7 method call,
```

## reference
### API docs
+ [`StreamReader.ReadToEnd Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readtoend?view=net-8.0)