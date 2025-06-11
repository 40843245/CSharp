# CH11 -- get current encoding of stream reader
## objectives
You will learn how to

+ get current encoding of stream reader

## CH11.1 -- get current encoding of stream reader
### `CurrentEncoding` getter property
returns the current character encoding that `StreamReader` instance is using at present.

## examples
### example 1
#### main coded
Invoking the following method

```
       /// <summary>
       /// illustrate how to
       /// 
       /// + read a file with different encodings using `System.IO.StreamReader`
       /// 
       /// + check the current encoding of the stream using
       ///  
       /// `System.IO.StreamReader.CurrentEncoding` instance getter-property
       /// </summary>
       public static void TestMethod17()
       {
           Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
           string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
           string fileName = "Words.txt";
           string filePath = System.IO.Path.Combine(inputDirectory , fileName);
           
           int nextCharCode = default;
           char nextChar = default;

           int counter = 1;

           ///* ----------- Example 1 ----------- *///
           Console.WriteLine("///* ----------- Example {0} ----------- *///" ,counter);
           using(
               StreamReader streamReader = 
                   new StreamReader(
                       filePath,
                       new UnicodeEncoding()
                   )
           )
           {
               Console.ForegroundColor = ConsoleColor.Green;
               Console.WriteLine("Current encoding: {0}" , streamReader.CurrentEncoding.EncodingName);
               Console.ForegroundColor = ConsoleColor.White;

               nextCharCode = streamReader.Read();
               nextChar = (char)nextCharCode;
               
               Console.WriteLine("next char:`{0}`, next char code:`{1}`" , nextChar.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r") , nextCharCode);

               Console.ForegroundColor = ConsoleColor.Green;
               Console.WriteLine("Current encoding: {0}" , streamReader.CurrentEncoding.EncodingName);
               Console.ForegroundColor = ConsoleColor.White;
           }
           counter++;

           ///* ----------- Example 2 ----------- *///
           Console.WriteLine("///* ----------- Example {0} ----------- *///" , counter);
           using(
               StreamReader streamReader =
                   new StreamReader(
                       filePath ,
                       Encoding.ASCII
                   )
           )
           {
               Console.ForegroundColor = ConsoleColor.Green;
               Console.WriteLine("Current encoding: {0}" , streamReader.CurrentEncoding.EncodingName);
               Console.ForegroundColor = ConsoleColor.White;

               nextCharCode = streamReader.Read();
               nextChar = (char)nextCharCode;

               Console.WriteLine("next char:`{0}`, next char code:`{1}`" , nextChar.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r") , nextCharCode);

               Console.ForegroundColor = ConsoleColor.Green;
               Console.WriteLine("Current encoding: {0}" , streamReader.CurrentEncoding.EncodingName);
               Console.ForegroundColor = ConsoleColor.White;
           }
           counter++;

           Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
       }
```

will output following

[`Output of console in TestMethod17`](./Output%20of%20console%20in%20TestMethod17.docx)

## reference
### API docs
+ [`StreamReader.CurrentEncoding Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.currentencoding?view=net-8.0)