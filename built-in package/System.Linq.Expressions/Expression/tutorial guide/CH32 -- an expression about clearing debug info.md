# CH32 -- an expression about clearing debug info
## objectives
You will learn

+ an expression about clearing debug info

## CH32.1 -- an expression about clearing debug info
### `Expression.ClearDebugInfo`
creates a DebugInfoExpression for clearing a sequence point.

## examples
### example 1
Modifies the example 1 in CH30.

#### code for Utility class

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

        public string Message { get; set; } = string.Empty;

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
                Message = stringFromstringBuilder;    
            }
            catch(Exception ex)
            {
                Logger.Error(ex , "Goodbye cruel world");
            }
        }

        public void LoggerInfo()
        {
            Logger.Info(Message);
        }

        public void LoggerError()
        {
            Logger.Error(Message);
        }

        public void Clear()
        {
            Message = string.Empty;
        }
    }
}
```

#### code for extension methods

`Apply` extension methods is defined in `HighOrderFunctionsExtensionMethods` static class.

`HighOrderFunctionsExtensionMethods.cs`

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

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.DebugInfo` and `Expression.ClearDebugInfo`.
        /// </summary>
        public static void TestMethod50()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            DebugErrorInfoGenerator debugInfoGenerator = new DebugErrorInfoGenerator();

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

            Console.WriteLine("After marking SequencePoint,");
            Console.WriteLine($"Logger Message:{debugInfoGenerator.Message}");
            Console.WriteLine("------------- Calling `LoggerInfo` instance method -------------");
            debugInfoGenerator.LoggerInfo();

            Console.WriteLine("------------- Calling `LoggerError` instance method -------------");
            debugInfoGenerator.LoggerError();
            
            debugInfoGenerator.Clear();

            Console.WriteLine("After clear Logger Message by `Clear` instance method call,");
            Console.WriteLine($"Logger Message:{debugInfoGenerator.Message}");
            Console.WriteLine("------------- Calling `LoggerInfo` instance method -------------");
            debugInfoGenerator.LoggerInfo();

            Console.WriteLine("------------- Calling `LoggerError` instance method -------------");

            debugInfoGenerator.MarkSequencePoint(lambdaExpression , 0 , debugInfoExpression);

            Console.WriteLine("After marking SequencePoint again,");
            Console.WriteLine($"Logger Message:{debugInfoGenerator.Message}");
            Console.WriteLine("------------- Calling `LoggerInfo` instance method -------------");
            debugInfoGenerator.LoggerInfo();

            Console.WriteLine("------------- Calling `LoggerError` instance method -------------");
            debugInfoGenerator.LoggerError();

            DebugInfoExpression clearDebugInfoExpression = Expression.ClearDebugInfo(document);

            Console.WriteLine("The DebugInfoExpression:`{0}`\nis for clearing a sequence point?\n`{1}`" , clearDebugInfoExpression .ToString(), clearDebugInfoExpression.IsClear);

            var clearDebugInfoLambdaExpression = 
                Expression.Lambda(clearDebugInfoExpression);
            clearDebugInfoLambdaExpression.Compile().DynamicInvoke();

            Console.WriteLine("After clear debug info,");
            Console.WriteLine($"Logger Message:{debugInfoGenerator.Message}");
            Console.WriteLine("------------- Calling `LoggerInfo` instance method -------------");
            debugInfoGenerator.LoggerInfo();

            Console.WriteLine("------------- Calling `LoggerError` instance method -------------");
            debugInfoGenerator.LoggerError();
        }
```

will output following

```
In TestMethod50 method call,
After marking SequencePoint,
Logger Message:1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
------------- Calling `LoggerInfo` instance method -------------
2025-05-26 11:34:29.2193|INFO|Example.Utilities.Generator.DebugErrorInfoGenerator|1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
------------- Calling `LoggerError` instance method -------------
2025-05-26 11:34:29.2713|ERROR|Example.Utilities.Generator.DebugErrorInfoGenerator|1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
After clear Logger Message by `Clear` instance method call,
Logger Message:
------------- Calling `LoggerInfo` instance method -------------
2025-05-26 11:34:29.2713|INFO|Example.Utilities.Generator.DebugErrorInfoGenerator|
------------- Calling `LoggerError` instance method -------------
After marking SequencePoint again,
Logger Message:1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
------------- Calling `LoggerInfo` instance method -------------
2025-05-26 11:34:29.2713|INFO|Example.Utilities.Generator.DebugErrorInfoGenerator|1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
------------- Calling `LoggerError` instance method -------------
2025-05-26 11:34:29.2713|ERROR|Example.Utilities.Generator.DebugErrorInfoGenerator|1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
The DebugInfoExpression:`<DebugInfo(/AppData/User.cs: 16707566, 0, 16707566, 0)>`
is for clearing a sequence point?
`True`
After clear debug info,
Logger Message:1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
------------- Calling `LoggerInfo` instance method -------------
2025-05-26 11:34:29.2713|INFO|Example.Utilities.Generator.DebugErrorInfoGenerator|1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
------------- Calling `LoggerError` instance method -------------
2025-05-26 11:34:29.2713|ERROR|Example.Utilities.Generator.DebugErrorInfoGenerator|1th parameter: arglambdaExpression.ReturnType.FullName: System.Int32
```

## reference
### API docs
+ [`Best practices for using NLog`](https://github.com/NLog/NLog/wiki/Tutorial#best-practices-for-using-nlog)
+ [`Expression.DebugInfo(SymbolDocumentInfo, Int32, Int32, Int32, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.debuginfo?view=net-8.0)
+ [`Expression.ClearDebugInfo(SymbolDocumentInfo) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.cleardebuginfo?view=net-8.0)
+ [`DebugInfoExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.debuginfoexpression?view=net-8.0)
+ [`SymbolDocumentInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.symboldocumentinfo?view=net-8.0)