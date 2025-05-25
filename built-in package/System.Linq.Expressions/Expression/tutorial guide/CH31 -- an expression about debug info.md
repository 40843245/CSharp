# CH31 -- an expression about debug info
## objectives
You will learn how to

+ create an expression containing debug info

## CH31.1 -- create an expression containing debug info
### `Expression.DebugInfo` static method
To create an expression containing debug info, simply invoke `Expression.DebugInfo` static method.

```
            string sourceFileFullPath = $@"/AppData/User.cs"; // full path of a file in `C#`.

            SymbolDocumentInfo document = Expression.SymbolDocument(sourceFileFullPath);

            DebugInfoExpression debugInfoExpression = Expression.DebugInfo(
                document: document ,
                startLine: 5 ,
                startColumn: 1 ,
                endLine: 5 ,
                endColumn: 10
            );
```

## examples
### example 1
#### uses
This example uses these packages:

+ `NLog` third package

This example uses these class

+ `DebugInfoGenerator` abstract class under `System.Runtime.CompilerServices` namespace

#### instruction to reproduce this example with new empty project
If you want reproduce this example with new empty project, you'll need to follow these steps.

##### Part 1 -- install necessary packages

Step 1:

Install [`NLog` third package](https://nlog-project.org/download/) in version `5.4.0`. 

<img width="659" alt="image" src="https://github.com/user-attachments/assets/a36f80ae-c79a-4e2d-82d9-ff5c94942f7c" />

##### Part 2 -- set up `NLog` configuration target for logging
For clarity, I don't put the code about `NLog` configuration target for logging in `Program.cs` because it will be messy and hard to read. 

Instead, I extract these code into `NLogInitializer` static classs in `NLogInitializer.cs`.

Step 1:

Create a `Startup` folder at top level.

Step 2:

Create `NLogInitializer.cs` under `Startup` folder.

<img width="263" alt="image" src="https://github.com/user-attachments/assets/25e81878-ce37-4ba5-b31f-96215df1d226" />

Step 3:

Define a static method named `Initialize` and set up `NLog` configuration target for logging inside this method

```
using Example.Globals.GlobalsFullPath;
using NLog;

namespace Example.Startup
{
    public static class NLogInitializer 
    {
        private static string fileName = GlobalsFileFullPath.NLOG_FILE_FULL_PATH; // full path of the file that may be written into when logging with `NLog`.
        public static void Initialize()
        {
            NLog.LogManager.Setup().LoadConfiguration(builder => {
                builder.ForLogger().FilterMinLevel(LogLevel.Info).WriteToConsole();
                builder.ForLogger().FilterMinLevel(LogLevel.Debug).WriteToFile(fileName: fileName);
            });
        }
    }
}
```

<img width="608" alt="image" src="https://github.com/user-attachments/assets/5cb933ac-5994-4c1f-87b5-e14080a69821" />

For more details about `Configure NLog Targets for output` , see [`Configure NLog Targets for output in .NET Framework`](https://github.com/NLog/NLog/wiki/Tutorial#configure-nlog-targets-for-output)

The above code is reference by [`Configure NLog Targets for output in .NET Framework`](https://github.com/NLog/NLog/wiki/Tutorial#configure-nlog-targets-for-output)

<img width="629" alt="image" src="https://github.com/user-attachments/assets/70a77ff6-7247-490f-a83c-2fcee3ccfa0a" />

##### Part 3 -- define a class that inherits `System.Runtime.CompilerServices.DebugInfoGenerator` abstract class
Since `DebugInfoGenerator` under `System.Runtime.CompilerServices` is an abstract class, it is NOT allowed to instantiate it.

Instead it is need to define a class that inherits `DebugInfoGenerator` class and implements its abstract method (i.e. method with `abstract` modifier).

Step 1:

Define a class named `DebugErrorInfoGenerator` that inherits `DebugInfoGenerator` class and implements its abstract method

```
public class DebugErrorInfoGenerator : DebugInfoGenerator
{
        public override void MarkSequencePoint(
            LambdaExpression lambdaExpression, 
            int ilOffset , 
            DebugInfoExpression sequencePoint
        )
        {
          // ... logic
        }
}
```

Step 2: 

We want to log the parameters of `lambdaExpression` which will return type `ReadOnlyCollection<ParameterExpression>` and its full name of return type in `lambdaExpression` with `NLog`.

And since `Logger.Info` static method only accepts one argument which MUST be a string. 

The string will be considered as info and Logger will log it into `NLog` targets.

We have to 

  + get parameters of `lambdaExpression` and join it to string
  + get its full name of return type in `lambdaExpression` and convert it to string.

Let's cooking.

1. For first small part get parameters of `lambdaExpression` and join it to string.

Since `ReadOnlyCollection<T>` has no higher-order methods (such as `ForEach` in `List<T>`),

we have to iterate it one-by-one and making it as a string.

Although, we can directly write code in `MarkSequencePoint` overriden method, for example

```
        public override void MarkSequencePoint(
            LambdaExpression lambdaExpression, 
            int ilOffset , 
            DebugInfoExpression sequencePoint
        )
        {
           // get the parameters of `lambdaExpression`
           var parameters = lambdaExpression.Parameters;
           var textList = new List<string>();
           foreach(var parameter in parameters)
           {
              textList.Add(parameter.ToString());
           }
           text = string.Join(System.Environment.NewLine,textList);
            // get its full name of return type in lambdaExpression`
            // ... logic

            // ... logging
        }
```

it's so messy so that the author decides to divide this functionality into two smaller functionality.

+ In first functionality,

we have to iterate element in `ReadOnlyCollection<T>` one-by-one to make it a list with type `List<T>`

and convert each element to string, making a list with type `List<string>`.

Simply said:

`ReadOnlyCollection<T>` => `List<string>`


    - For clarity, the author extracts it into an extension method.
    - For generic use of any type, the author makes it generic (an extension method with generics).
    - For flexibility (**NOT ONLY** it can convert the element (with type `ParameterExpression`) into string and then add it into a list in the defined extension method

**BUT ALSO** it can do lots of things (invoking a parameterless function)), the author refactors the code snippets.

Although its take a while (about 20 minutes), it is worth to do that (for the author) since it is flexible, which can be used in a parameterless function.

The refactored code is as follows

```
    public static partial class HighOrderFunctionsExtensionMethods
    {
        public static List<TFunctionResult> Apply<TElement, TFunctionResult>(
            this ICollection<TElement> collection,
            Func<TElement , TFunctionResult> func
        )
        {
            List<TFunctionResult> list = new List<TFunctionResult>();
            foreach(var coll in collection)
            {
                var result = func.Invoke(coll);
                list.Add(result);
            }
            return list;
        }
    }
```

And I can easily invoke the extension method to convert `ReadOnlyCollection<T>` to `List<string>` as follows.

```
                // textList is type `List<string>`

                int counter = 0;
                var formattingString = "{0}th parameter: {1}";

                var parameters = lambdaExpression.Parameters;
                textList = parameters.Apply(
                  (parameter) => {
                      counter++;
                      var result = string.Format(formattingString, counter, parameter.ToString());
                      return result;
                  }
                );
```

+ In second functionality, we just need to join the list of string `List<string>` to string.

Thanks `C#` developer, there is a built-in high-order method -- `string.Join` static method.

we can simply do that 

```
text = string.Join(System.Environment.NewLine,textList);
```

2. For second small part, get its full name of return type in `lambdaExpression` and convert it to string.

we can simply do that by accessing `lambdaExpression.ReturnType.FullName` getter-property.

3. Lastly, we want to log it into logger,

So, we need to get the current logger.

The simplest way to do that is to invoke `NLog.LogManager.GetCurrentClassLogger()`

```
private static readonly NLog.Logger Logger = NLog.LogManager.GetCurrentClassLogger();
```

Combining them together, we complete the `DebugErrorInfoGenerator` class.

`DebugErrorInfoGenerator.cs`

```
using System;
using System.Collections.Generic;
using System.Linq.Expressions;
using System.Runtime.CompilerServices;
using System.Text;
using static Example.Extensions.ExtensionMethods.HighOrderFunctionsExtensionMethods.HighOrderFunctionsExtensionMethods;

namespace Example.Utilities.Generator
{
    public class DebugErrorInfoGenerator : DebugInfoGenerator
    {
        private static readonly NLog.Logger Logger = NLog.LogManager.GetCurrentClassLogger();
        public override void MarkSequencePoint(
            LambdaExpression lambdaExpression, 
            int ilOffset , 
            DebugInfoExpression sequencePoint
        )
        {
            try
            {
                StringBuilder stringBuilder = new StringBuilder();
                string stringFromstringBuilder = string.Empty;
                List<string> textList = new List<string>();
                string text = string.Empty;
                int counter = 0;
                var formattingString = "{0}th parameter: {1}";

                var parameters = lambdaExpression.Parameters;
                textList = parameters.Apply(
                  (parameter) => {
                      counter++;
                      var result = string.Format(formattingString, counter, parameter.ToString());
                      return result;
                  }
                );

                text = string.Join(System.Environment.NewLine,textList);

                stringBuilder.Append(text);

                text = string.Empty;
                textList.Clear();
                counter = 0;
                formattingString = "lambdaExpression.ReturnType.FullName: {0}";

                text = string.Format(formattingString,lambdaExpression.ReturnType.FullName);

                stringBuilder.Append(text);

                stringFromstringBuilder = stringBuilder.ToString();
                Logger.Info(stringFromstringBuilder);
                System.Console.ReadKey();
            }
            catch(Exception ex)
            {
                Logger.Error(ex , "Goodbye cruel world");
            }
        }
    }
}
```

##### Part 4 -- Instantiate the instance with our defined class (that inherits `DebugInfoGenerator`) and use it
To instantiate the instance with our defined class (that inherits `DebugInfoGenerator`), 

simply use `new` keyword.

```
DebugInfoGenerator debugInfoGenerator = new DebugErrorInfoGenerator();
```

Then we can invoke `MarkSequencePoint` instance method

```
// ... `debugInfoExpression` is type `DebugInfoExpression`
debugInfoGenerator.MarkSequencePoint(lambdaExpression , 0 , debugInfoExpression);
```

#### full code of helper methods and extension methods
That following figures illustrates the structure of project and all files that are necessary to be used (with highlihgted by red pen).

<img width="959" alt="image" src="https://github.com/user-attachments/assets/4e33cb95-8ad6-4438-b88f-46063267c393" />

`Apply.cs`

```
using System;
using System.Collections.Generic;

namespace Example.Extensions.ExtensionMethods.HighOrderFunctionsExtensionMethods
{
    public static partial class HighOrderFunctionsExtensionMethods
    {
        public static List<TFunctionResult> Apply<TElement, TFunctionResult>(
            this ICollection<TElement> collection,
            Func<TElement , TFunctionResult> func
        )
        {
            List<TFunctionResult> list = new List<TFunctionResult>();
            foreach(var coll in collection)
            {
                var result = func.Invoke(coll);
                list.Add(result);
            }
            return list;
        }
    }
}
```

`NLogInitializer.cs`

```
using Example.Globals.GlobalsFullPath;
using NLog;

namespace Example.Startup
{
    public static class NLogInitializer 
    {
        private static string fileName = GlobalsFileFullPath.NLOG_FILE_FULL_PATH;
        public static void Initialize()
        {
            NLog.LogManager.Setup().LoadConfiguration(builder => {
                builder.ForLogger().FilterMinLevel(LogLevel.Info).WriteToConsole();
                builder.ForLogger().FilterMinLevel(LogLevel.Debug).WriteToFile(fileName: fileName);
            });
        }
    }
}
```

#### script as test data
`User.cs`

```
namespace Example.AppData
{
    public class User
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

#### main method
Invoking this method 

```
        /// <summary>
        /// illustrate writing data about expression info as debug info with `DebugInfoGenerator` in `NLog` third package. 
        /// </summary>
        public static void TestMethod15()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            DebugInfoGenerator debugInfoGenerator = new DebugErrorInfoGenerator();

            ParameterExpression parameterExpression = Expression.Parameter(typeof(int) , "arg");

            // This expression represents a lambda expression
            // that adds 1 to the parameter value.
            LambdaExpression lambdaExpression = Expression.Lambda(
                Expression.Add(
                    parameterExpression ,
                    Expression.Constant(1)
                ) ,
                new List<ParameterExpression>() { parameterExpression }
            );

            string sourceFileFullPath = $@"/AppData/User.cs";
            SymbolDocumentInfo document = Expression.SymbolDocument(sourceFileFullPath);

            DebugInfoExpression debugInfoExpression = Expression.DebugInfo(
                document: document ,
                startLine: 5 ,
                startColumn: 1 ,
                endLine: 5 ,
                endColumn: 10
            );

            debugInfoGenerator.MarkSequencePoint(lambdaExpression , 0 , debugInfoExpression);
        }
```

will output like following in console.

<img width="870" alt="image" src="https://github.com/user-attachments/assets/8817bb47-82a3-4d31-ac2f-78b26ec08974" />
