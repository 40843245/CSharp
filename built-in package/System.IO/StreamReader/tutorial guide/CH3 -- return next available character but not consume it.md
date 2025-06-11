# CH3 -- return next available character  but not consume it
## objectives
You will learn how to

+ return next available character but not consume it

## CH3.1 -- return next available character but not consume it
### `Peek` instance method
It will return next available character (`int` type) but not consume it.

If there are no characters to be read, it will return -1.

If the stream does not support seeking, it will also return -1.

The following code snippet also can read file content line-by-line.

```
using(StreamReader streamReader = new StreamReader(filePath))
{
    while(streamReader.Peek() > -1)
    {
        string line = streamReader.ReadLine();
        Console.WriteLine(line);
    }
}
```

## examples 
### example 1
#### main code
The following example will read the file content from 

`~/AppData/DataSource/Input/Words.txt` into a stream reader.

The print it on console line-by-line.

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
        /// + seek there are available character to be read 
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                while(streamReader.Peek() > -1)
                {
                    string line = streamReader.ReadLine();
                    Console.WriteLine(line);
                }
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod2 method call,
Hello
World
A
File
Contain
A
List
Of
Words
End of TestMethod2 method call,
```

### example 2
#### main code
The following example will read the file content from 

`~/AppData/DataSource/Input/Words.txt` into a stream reader.

The print the first char of the line to be read, its char code and the line to be read on console line-by-line.

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
        /// + seek there are available character to be read 
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                int nextCharCode = -1;
                while((nextCharCode = streamReader.Peek()) > -1)
                {
                    char nextChar = (char)nextCharCode;
                    string line = streamReader.ReadLine();
                    Console.WriteLine("next char:`{0}` next char code:`{1}`,next line:{2}",nextChar,nextCharCode,line);
                }
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod3 method call,
next char:`H` next char code:`72`,next line:Hello
next char:`W` next char code:`87`,next line:World
next char:`A` next char code:`65`,next line:A
next char:`F` next char code:`70`,next line:File
next char:`C` next char code:`67`,next line:Contain
next char:`A` next char code:`65`,next line:A
next char:`L` next char code:`76`,next line:List
next char:`O` next char code:`79`,next line:Of
next char:`W` next char code:`87`,next line:Words
End of TestMethod3 method call,
```

## reference
### API docs
+ [`StreamReader.Peek Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.peek?view=net-8.0)

+ [`StreamReader.ReadLine Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.readline?view=net-8.0)

