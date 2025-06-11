# CH9 -- detect whether the end of stream has been reached
## objectives
You will learn how to

+ detect whether the end of stream has been reached

## CH9.1 -- detect whether the end of stream has been reached
### `EndOfStream` getter property
returns true iff the end of stream has been reached.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + detect whether the end of stream has been reached using `System.IO.StreamReader.EndOfStream` instance getter-property.
        /// </summary>
        public static void TestMethod15()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                bool isEndOfStream = false;
                isEndOfStream = streamReader.EndOfStream;
                Console.WriteLine("Is end of stream? {0}" , isEndOfStream);
                streamReader.ReadToEnd();
                isEndOfStream = streamReader.EndOfStream;
                Console.WriteLine("Is end of stream? {0}" , isEndOfStream);
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod15 method call,
Is end of stream? False
Is end of stream? True
End of TestMethod15 method call,
```

## reference
### API docs
+ [`StreamReader.EndOfStream Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.endofstream?view=net-8.0)