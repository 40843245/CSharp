# CH6 -- underlying stream of stream writer
## objectives
You will learn how to

+ get underlying stream of stream writer

## CH6.1 -- underlying stream of stream writer
### `BaseStream` getter property

```
Stream baseStream = streamWriter.BaseStream;
```

will get underlying stream of `streamWriter` instance.

### Remarks
> [!WARNING]
> to get that freshest info of underlying stream 
> 
> (such as Length through `streamWriter.BaseStream.Length`,
> 
> Position through `streamWriter.BaseStream.Position`)
> 
> after an operation underlying stream 
> 
> (such as set position of underlying stream back to the beginning 
> 
> through `streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin)`,
> 
> we need to flush the buffer (such as through `streamWriter.Flush()` method call)

For fully understanding, 

see following code, 

then compare generated `output ex1.txt` and `output ex2.txt` files in example 1.

## examples
### example 1
#### helper class
##### `TextHelper` class
wrapped into a class that can print texts to `Console` 

then write same texts into `StreamWriter`

see demo project for complete code.

#### main code
Invoking following method will 

1. create directory `TestMethod6` under `~/AppData/DataSource/Output` directory (if it does NOT exist).

2. create file `output ex1.txt` and `output ex2.txt` under `~/AppData/DataSource/Output/TestMethod6` (if it does NOT exist).

3. then write (,or overwrite) text into those files as follows

+ 1th part:

It will generate file `output ex1.txt` (if it does NOT exist),

and then print texts to `Console` and write same texts into `output ex1.txt`.

With the file content and output on `Console`,

it can explain that

    - If one does flush the buffer of `StreamWriter` instance first
    
    (such as through `streamWriter.Flush()` method call),

    then one can NOT fetch the freshest value in the property of underlying stream 

    (such as Length through `streamWriter.BaseStream.Length`,
 
    Position through `streamWriter.BaseStream.Position`)

    about operation of underlying stream

    (such as set position of underlying stream back to the beginning 
    
    through `streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin)` 

    and the operation is also invalid so that

    it will perform an operation about `StreamWriter` instance 

    (such as `streamWriter.WriteLine()`) with old value.

In the following part,

one does NOT flush the buffer, 

so `streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin)` statement 

will NOT update `streamWriter.BaseStream.Position` and `streamWriter.BaseStream.Length`,

Furthermore, it will NOT append the text from the beginning of the `streamWriter`.

`output ex1.txt`

```
Current Position is `0`
Current Length is `0`
Ayase Eli:Welcome to join LoveLive club
Ayase Eli:Let's say Hello to members in LoveLive club.

Huang Jay:Hello Yazawa Nico.
Huang Jay:Hello Kosaka Honoka.
Huang Jay:Hello Ayase Eli.
Huang Jay:Hello Minami Kotori.
Huang Jay:Hello Sonoda Umi.
Huang Jay:Hello Hoshizora Rin.
Huang Jay:Hello Nishikino Maki.
Huang Jay:Hello Tojo Nozomi.
Huang Jay:Hello Koizumi Hanayo.
And more...
Before backing to beginning with offset 0,
Current Position is `0`
Current Length is `0`
After backing to beginning with offset 0,
Current Position is `0`
Current Length is `0`
Ayase Eli:Please introduce yourself.
Huang Jay:My name is Huang Jay

```

output on `Console`

[output of console of Part 1 in TestMethod6](./output%20of%20console%20of%20Part%201%20in%20TestMethod6.docx)

+ 2th part:

It will generate file `output ex2.txt` (if it does NOT exist),

and then print texts to `Console` and write same texts into `output ex2.txt`.

With the file content and output on `Console`,

it can prove that

    - To get that freshest info of underlying stream 
       
       (such as Length through `streamWriter.BaseStream.Length`,
       
       Position through `streamWriter.BaseStream.Position`)
       
       after an operation underlying stream 
       
       (such as set position of underlying stream back to the beginning 
       
       through `streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin)`,
       
       we need to flush the buffer (such as through `streamWriter.Flush()` method call)

`output ex2.txt`

```
Before backing to beginning with offset 0,
Current Position is `439`
Current Length is `439`
After backing to beginning with offset 0,
Current Position is `0`
Current Length is `439`
Ayase Eli:Please introduce yourself.
Huang Jay:My name is Huang Jay
 Kotori.
Huang Jay:Hello Sonoda Umi.
Huang Jay:Hello Hoshizora Rin.
Huang Jay:Hello Nishikino Maki.
Huang Jay:Hello Tojo Nozomi.
Huang Jay:Hello Koizumi Hanayo.
And more...

```

output on `Console`

[output of console of Part 2 in TestMethod6](./output%20of%20console%20of%20Part%202%20in%20TestMethod6.docx)

+ method

```
       <summary>
       illustrate how to
       
       + get info of underlying stream through 
       
       accessing the property of `BaseStream` instance getter property.
       
       And also prove that
       
       + to get that freshest info of underlying stream 
       
       (such as Length through `streamWriter.BaseStream.Length`,
       
       Position through `streamWriter.BaseStream.Position`)
       
       after an operation underlying stream 
       
       (such as set position of underlying stream back to the beginning 
       
       through `streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin)`,
       
       we need to flush the buffer (such as through `streamWriter.Flush()` method call)
       </summary>
        public static void TestMethod6()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string outputFilePath;
            string outputFileName;
            string formattingString;
            string studentCouncilNameAtLoveLiveSchool = "Ayase Eli";
            string newMemberName = "Huang Jay";
            string testMethodName = MethodBase.GetCurrentMethod().Name;
            string testMethodDirectory = System.IO.Path.Combine(Constants.PathConstants.DirectoryConstants.OUTPUT , testMethodName);

            int status = DirectoryHandler.CreateDirectory(testMethodDirectory);
            if(status == -1)
            {
                return;
            }

            int counter = 1;
            formattingString = "output ex{0}.txt";
            studentCouncilNameAtLoveLiveSchool = "Ayase Eli";
            newMemberName = "Huang Jay";
            TextHelper textHelper;

          * --------------- Example 1 --------------- *///
            Console.WriteLine("///* --------------- Example {0} --------------- *///",counter);
            outputFileName = string.Format(formattingString , counter);
            outputFilePath = System.IO.Path.Combine(testMethodDirectory , outputFileName);
            FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist

            using(
                StreamWriter streamWriter =
                    new StreamWriter(
                        outputFilePath ,
                        false
                    )
            )
            {
                streamWriter.AutoFlush = false;

                textHelper = new TextHelper()
                {
                    streamWriter = streamWriter
                };

                Console.ForegroundColor = ConsoleColor.Green;

                textHelper.WriteLine("Current Position is `{0}`",streamWriter.BaseStream.Position);
                textHelper.WriteLine("Current Length is `{0}`",streamWriter.BaseStream.Length);

                Console.ForegroundColor = ConsoleColor.White;

                textHelper.WriteLine("{0}:Welcome to join LoveLive club" , studentCouncilNameAtLoveLiveSchool);
                textHelper.WriteLine("{0}:Let's say Hello to members in LoveLive club.", studentCouncilNameAtLoveLiveSchool);
                textHelper.WriteLine();
                textHelper.WriteLine("{0}:Hello Yazawa Nico.", newMemberName);
                textHelper.WriteLine("{0}:Hello Kosaka Honoka." , newMemberName);
                textHelper.WriteLine("{0}:Hello Ayase Eli." , newMemberName);
                textHelper.WriteLine("{0}:Hello Minami Kotori.",newMemberName);
                textHelper.WriteLine("{0}:Hello Sonoda Umi." , newMemberName);
                textHelper.WriteLine("{0}:Hello Hoshizora Rin." , newMemberName);
                textHelper.WriteLine("{0}:Hello Nishikino Maki." , newMemberName);
                textHelper.WriteLine("{0}:Hello Tojo Nozomi." , newMemberName);
                textHelper.WriteLine("{0}:Hello Koizumi Hanayo." , newMemberName);
                textHelper.WriteLine("And more...");

                textHelper.WriteLine("Before backing to beginning with offset 0,");

                Console.ForegroundColor = ConsoleColor.Green;

                textHelper.WriteLine("Current Position is `{0}`" , streamWriter.BaseStream.Position);
                textHelper.WriteLine("Current Length is `{0}`" , streamWriter.BaseStream.Length);

                Console.ForegroundColor = ConsoleColor.White;

                streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin);

                textHelper.WriteLine("After backing to beginning with offset 0,");

                Console.ForegroundColor = ConsoleColor.Green;

                textHelper.WriteLine("Current Position is `{0}`" , streamWriter.BaseStream.Position);
                textHelper.WriteLine("Current Length is `{0}`" , streamWriter.BaseStream.Length);

                Console.ForegroundColor = ConsoleColor.White;

                textHelper.WriteLine("{0}:Please introduce yourself." , studentCouncilNameAtLoveLiveSchool);
                textHelper.WriteLine("{0}:My name is {0}" , newMemberName);
            }

            counter++;

          * --------------- Example 2 --------------- *///
            Console.WriteLine("///* --------------- Example {0} --------------- *///" , counter);
            outputFileName = string.Format(formattingString , counter);
            outputFilePath = System.IO.Path.Combine(testMethodDirectory , outputFileName);
            FileHandler.CreateFile(outputFilePath); // Create the file if it does not exist

            using(
                StreamWriter streamWriter =
                    new StreamWriter(
                        outputFilePath ,
                        false
                    )
            )
            {
                streamWriter.AutoFlush = false;

                textHelper = new TextHelper()
                {
                    streamWriter = streamWriter
                };

                Console.ForegroundColor = ConsoleColor.Green;

                textHelper.WriteLine("Current Position is `{0}`" , streamWriter.BaseStream.Position);
                textHelper.WriteLine("Current Length is `{0}`" , streamWriter.BaseStream.Length);

                Console.ForegroundColor = ConsoleColor.White;

                textHelper.WriteLine("{0}:Welcome to join LoveLive club" , studentCouncilNameAtLoveLiveSchool);
                textHelper.WriteLine("{0}:Let's say Hello to members in LoveLive club." , studentCouncilNameAtLoveLiveSchool);
                textHelper.WriteLine();
                textHelper.WriteLine("{0}:Hello Yazawa Nico." , newMemberName);
                textHelper.WriteLine("{0}:Hello Kosaka Honoka." , newMemberName);
                textHelper.WriteLine("{0}:Hello Ayase Eli." , newMemberName);
                textHelper.WriteLine("{0}:Hello Minami Kotori." , newMemberName);
                textHelper.WriteLine("{0}:Hello Sonoda Umi." , newMemberName);
                textHelper.WriteLine("{0}:Hello Hoshizora Rin." , newMemberName);
                textHelper.WriteLine("{0}:Hello Nishikino Maki." , newMemberName);
                textHelper.WriteLine("{0}:Hello Tojo Nozomi." , newMemberName);
                textHelper.WriteLine("{0}:Hello Koizumi Hanayo." , newMemberName);
                textHelper.WriteLine("And more...");

                streamWriter.Flush();
                
                textHelper.WriteLine("Before backing to beginning with offset 0,");

                Console.ForegroundColor = ConsoleColor.Green;

                textHelper.WriteLine("Current Position is `{0}`" , streamWriter.BaseStream.Position);
                textHelper.WriteLine("Current Length is `{0}`" , streamWriter.BaseStream.Length);

                Console.ForegroundColor = ConsoleColor.White;

                streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin);

                textHelper.WriteLine("After backing to beginning with offset 0,");

                Console.ForegroundColor = ConsoleColor.Green;

                textHelper.WriteLine("Current Position is `{0}`" , streamWriter.BaseStream.Position);
                textHelper.WriteLine("Current Length is `{0}`" , streamWriter.BaseStream.Length);

                Console.ForegroundColor = ConsoleColor.White;

                textHelper.WriteLine("{0}:Please introduce yourself." , studentCouncilNameAtLoveLiveSchool);
                textHelper.WriteLine("{0}:My name is {0}" , newMemberName);
            }

            counter++;

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

The above code output following

[output of console in TestMethod6](./output%20of%20console%20in%20TestMethod6.docx)

## reference
### API docs
+ [`StreamWriter.BaseStream Property`](https://learn.microsoft.com/en-us/dotnet/api/system.io.streamwriter.basestream?view=net-8.0)

+ [`Stream Class`](https://learn.microsoft.com/en-us/dotnet/api/system.io.stream?view=net-8.0)

### AI model
#### Google Gemini
+ [Why does after executing `streamWriter.BaseStream.Seek(0 , SeekOrigin.Begin);` , the value of its underlying length (`streamWriter.BaseStream.Length`) and position `streamWriter.BaseStream.Position` are NOT updated?](https://g.co/gemini/share/5d35cbc5bd80)