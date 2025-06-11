# CH4 -- read charactes from stream reader
## objectives
You will learn how to

+ read charactes from stream reader

## CH4.1 -- read charactes from stream reader
### `Read` instance method
It can read one character or a set of character from position 

(that current pointer points to) and 

advance the position by the number of characters are read.

#### Overloads

+ `Read()`: 

will read one character from position 

(that current pointer points to) and 

advance the position by one character

then returns the char code of char that is read, or 

returns -1 iff there are no available char that can be read.


+ `Read(Span<Char> span)`:

will read a set of character (determining by capacity of span) from position 

(that current pointer points to) and 

advance the position by the capacity of span

then returns the number of characters to be read or

returns 0 iff the pointer points to the end of the stream and

there are no available char that can be read.

+ `Read(char[] buffer, int index, int count)`:

will read `count` characters from position 

(that current pointer points to) plus offset `index` and 

advance the position by the number of characters to be read

then returns the number of characters to be read or

returns 0 iff the pointer points to the end of the stream and

there are no available char that can be read.

### `ReadAsync` instance method

### difference between `StreamReader.Read` and `StreamReader.ReadBlock`
For their behaviour, the main difference between

`StreamReader.Read(char[] buffer, int index, int count)`

and 

`StreamReader.ReadBlock(char[] buffer, int index, int count)`

instance method is 

the reading is **sync** or **async**, or 

whether there is **no guarantee**.

+ `StreamReader.Read(char[] buffer, int index, int count)`:

**no guarantee** to fill the `count` characters. 

May return fewer characters than `count` if data is not immediately available.

+ `StreamReader.ReadBlock(char[] buffer, int index, int count)`

**guarantees** to read `count` characters, unless the end of the stream is reached.

For more details, see [What's the difference with `StreamReader.Read` and `StreamReader.ReadBlock`](https://docs.google.com/spreadsheets/d/1tLlLA_Dj6dpSHq1-qs7baLmPNvm2hEBGgd-Kvyfkp58/edit?usp=sharing)

and [What's the difference with `StreamReader.Read` and `StreamReader.ReadBlock`](https://g.co/gemini/share/10aaccbb934b)

## examples
### example 1
#### main code
The following example illustrates how to 

+ use `Read(Span<char> span)` overloaded method to read a set of characters from stream reader.

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
        /// + read a set of character from a file using `System.IO.StreamReader`.
        /// </summary>
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);

            int length = 10; // Define the length of the buffer
            Span<char> buffer = stackalloc char[length]; // Allocate a buffer on the stack

            using(StreamReader streamReader = new StreamReader(filePath))
            {
                int numberOfCharToRead = -1;
                while((numberOfCharToRead = streamReader.Read(buffer)) > 0)
                {
                    Console.WriteLine("read `{0}` char, these chars `{1}` to be read.", numberOfCharToRead , buffer.ToString().Replace("\n","\\n").Replace("\r","\\r"));
                }
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod4 method call,
read `10` char, these chars `Hello\r\nWor` to be read.
read `10` char, these chars `ld\r\nA\r\nFil` to be read.
read `10` char, these chars `e\r\nContain` to be read.
read `10` char, these chars `\r\nA\r\nList\r` to be read.
read `10` char, these chars `\nOf\r\nWords` to be read.
End of TestMethod4 method call,
```

### example 2
#### main code
The following example illustrates how to 

+ use `Read()` overloaded method to read one character from stream reader.

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
        /// + read one character from a file using `System.IO.StreamReader`.
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);
            using(StreamReader streamReader = new StreamReader(filePath))
            {
                int nextCharCode = -1;
                while((nextCharCode = streamReader.Read()) > -1)
                {
                    char nextChar = (char)nextCharCode;
                    string nextString = nextChar.ToString().Replace("\n" , "\\n").Replace("\r" , "\\r");
                    Console.WriteLine("next char:`{0}` next char code:`{1}`" , nextString , nextCharCode);
                }
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod5 method call,
next char:`H` next char code:`72`
next char:`e` next char code:`101`
next char:`l` next char code:`108`
next char:`l` next char code:`108`
next char:`o` next char code:`111`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`W` next char code:`87`
next char:`o` next char code:`111`
next char:`r` next char code:`114`
next char:`l` next char code:`108`
next char:`d` next char code:`100`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`A` next char code:`65`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`F` next char code:`70`
next char:`i` next char code:`105`
next char:`l` next char code:`108`
next char:`e` next char code:`101`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`C` next char code:`67`
next char:`o` next char code:`111`
next char:`n` next char code:`110`
next char:`t` next char code:`116`
next char:`a` next char code:`97`
next char:`i` next char code:`105`
next char:`n` next char code:`110`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`A` next char code:`65`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`L` next char code:`76`
next char:`i` next char code:`105`
next char:`s` next char code:`115`
next char:`t` next char code:`116`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`O` next char code:`79`
next char:`f` next char code:`102`
next char:`\r` next char code:`13`
next char:`\n` next char code:`10`
next char:`W` next char code:`87`
next char:`o` next char code:`111`
next char:`r` next char code:`114`
next char:`d` next char code:`100`
next char:`s` next char code:`115`
End of TestMethod5 method call,
```

### example 3
#### main code
The following example illustrates how to 

+ use `Read(Char[], Int32, Int32)` overloaded method to read more characters from stream reader.

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
        /// + read more characters from a file using `System.IO.StreamReader`.
        /// </summary>
        public static void TestMethod6()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            string inputDirectory = Example.Constants.PathConstants.DirectoryConstants.INPUT;
            string fileName = "Words.txt";
            string filePath = System.IO.Path.Combine(inputDirectory , fileName);
            using(StreamReader streamReader = new StreamReader(filePath))
            {
                int length = 10; // Define the length of the buffer
                int index = 0;
                int count = 0;
                int numberOfCharToRead = 0;
                char [ ] buffer = new char [length];
                for(
                    count = buffer.Length; 
                    (numberOfCharToRead = streamReader.Read(buffer,index,count))> 0;
                    count = buffer.Length
                )
                {
                    Console.WriteLine("read `{0}` char, these chars `{1}` to be read." , numberOfCharToRead , (new string(buffer)).Replace("\n" , "\\n").Replace("\r" , "\\r"));
                }
            }
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod6 method call,
read `10` char, these chars `Hello\r\nWor` to be read.
read `10` char, these chars `ld\r\nA\r\nFil` to be read.
read `10` char, these chars `e\r\nContain` to be read.
read `10` char, these chars `\r\nA\r\nList\r` to be read.
read `10` char, these chars `\nOf\r\nWords` to be read.
End of TestMethod6 method call,
```

## reference
### API docs
+ [`StreamReader.Read Method`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamreader.read?view=net-8.0)