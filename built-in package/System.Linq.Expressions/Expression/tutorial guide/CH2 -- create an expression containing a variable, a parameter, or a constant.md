# CH2 -- create an expression containing a variable, a parameter, or a constant
## objectives
You will learn

+ an expression containing a variable
+ an expression containing a parameter
+ an expression containing a constant
+ an expression containing a default value of the given type
+ an expression about assigment operator

## CH2.1 -- an expression containing a variable
### `Expression.Variable` static method call
You can create an expression contnaining a variable using `Expression.Variable` static method call.

```
ParameterExpression intParameterExpression1 = Expression.Variable(typeof(int) , "x");
```

In above code snippets, an expression containing a variable will be created. 

The variable name is `x` with int type.

For more details, see [`Expression.Variable Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.variable?view=net-8.0)

## CH2.2 -- an expression containing a parameter
### `Expression.Parameter` static method call
You can create an expression contnaining a variable using `Expression.Parameter` static method call.

```
ParameterExpression intParameterExpression1 = Expression.Parameter(typeof(int) , "x");
```

In above code snippets, an expression containing a parameter will be created. 

The parameter name is `x` with int type.

For more details, see [`Expression.Parameter Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.parameter?view=net-8.0)

## CH2.3 -- an expression containing a constant
### `Expression.Constant` static method call
You can create an expression contnaining a constant using `Expression.Constant` static method call.

```
ConstantExpression expressionConstantOne = Expression.Constant(1);
```

In above code snippets, an expression containing a constant `1` will be created. 

For more details, see [`Expression.Constant Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.constant?view=net-8.0)

## CH2.4 -- an expression containing a default value of the given type
### `Expression.Default` static method
You can create an expression containing a default value of the given type using `Expression.Default` static method call.

```
DefaultExpression boolDefaultExpression = Expression.Default(typeof(bool));
```

## CH2.5 -- an expression about assigment operator
### `Expression.Assign` static method
You can create an expression about assigment operator using `Expression.Assign` static method call.

```
BinaryExpression binaryExpression = Expression.Assign(
                                      Expression.Variable(typeof(int) , "x") ,
                                      Expression.Constant(42)
                                    );
```

## examples
### example 1
Invoking this method

```
        /// <summary>
        /// illustrate assignment operations.  
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            List<BinaryExpression> binaryExpressions = new List<BinaryExpression>();

            binaryExpressions.Add(
                Expression.Assign(
                    Expression.Variable(typeof(int) , "x") ,
                    Expression.Constant(42)
                )
            );

            Console.WriteLine("About `binaryExpressions`");
            binaryExpressions.ForEach(
                (binaryExpression) =>
                {
                    string infoText = binaryExpression.GetInfo();
                    Console.WriteLine(infoText);
                }
            );
            Console.WriteLine();
        }
```

where

`BinaryExpression.GetInfo` extension method is defined in `BinaryExpressionsExtensionMethods` static class.

`BinaryExpressionsExtensionMethods` static class in `ExpressionsExtensionMethods.cs`

```
public static class BinaryExpressionsExtensionMethods
    {
        public static string GetInfo(
            this BinaryExpression binaryExpression
        )
        {
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendFormat("binaryExpression.Left:{0}\n" , binaryExpression.Left);
            stringBuilder.AppendFormat("binaryExpression.Right:{0}\n" , binaryExpression.Right);
            stringBuilder.AppendFormat("binaryExpression.IsLifted:{0}\n" , binaryExpression.IsLifted);
            stringBuilder.AppendFormat("binaryExpression.IsLiftedToNull:{0}\n" , binaryExpression.IsLiftedToNull);
            return stringBuilder.ToString();
        }
    }
```

It will output

```
In TestMethod8 method call,
About `binaryExpressions`
binaryExpression.Left:x
binaryExpression.Right:42
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False


```

### example 2
Invoking this method

```
        /// <summary>
        /// illustrates an expression with default value.
        /// </summary>
        public static void TestMethod16()
        {
            string text = string.Empty;

            DefaultExpression boolDefaultExpression = Expression.Default(typeof(bool));
            text = boolDefaultExpression.GetInfo<bool>();
            Console.WriteLine(text);

            DefaultExpression byteDefaultExpression = Expression.Default(typeof(byte));
            text = byteDefaultExpression.GetInfo<byte>();
            Console.WriteLine(text);

            DefaultExpression shortDefaultExpression = Expression.Default(typeof(short));
            text = shortDefaultExpression.GetInfo<short>();
            Console.WriteLine(text);

            DefaultExpression int32DefaultExpression = Expression.Default(typeof(int));
            text = int32DefaultExpression.GetInfo<int>();
            Console.WriteLine(text);

            DefaultExpression longDefaultExpression = Expression.Default(typeof(long));
            text = longDefaultExpression.GetInfo<long>();
            Console.WriteLine(text);

            DefaultExpression ushortDefaultExpression = Expression.Default(typeof(ushort));
            text = ushortDefaultExpression.GetInfo<ushort>();
            Console.WriteLine(text);

            DefaultExpression uintDefaultExpression = Expression.Default(typeof(uint));
            text = uintDefaultExpression.GetInfo<uint>();
            Console.WriteLine(text);

            DefaultExpression ulongDefaultExpression = Expression.Default(typeof(ulong));
            text = ulongDefaultExpression.GetInfo<ulong>();
            Console.WriteLine(text);

            DefaultExpression floatDefaultExpression = Expression.Default(typeof(float));
            text = floatDefaultExpression.GetInfo<float>();
            Console.WriteLine(text);

            DefaultExpression doubleDefaultExpression = Expression.Default(typeof(double));
            text = doubleDefaultExpression.GetInfo<double>();
            Console.WriteLine(text);

            DefaultExpression charDefaultExpression = Expression.Default(typeof(char));
            text = charDefaultExpression.GetInfo<char>();
            Console.WriteLine(text);

            DefaultExpression stringDefaultExpression = Expression.Default(typeof(string));
            text = stringDefaultExpression.GetInfo<string>();
            Console.WriteLine(text);

            /// *---------------------- More complex type --------------------- *///
            
            // *---------------------- About Date and Time --------------------- *//
            DefaultExpression datetTimeDefaultExpression = Expression.Default(typeof(DateTime));
            text = datetTimeDefaultExpression.GetInfo<DateTime>();
            Console.WriteLine(text);

            DefaultExpression timeSpanDefaultExpression = Expression.Default(typeof(TimeSpan));
            text = timeSpanDefaultExpression.GetInfo<TimeSpan>();
            Console.WriteLine(text);

            DefaultExpression timeZoneDefaultExpression = Expression.Default(typeof(TimeZone));
            text = timeZoneDefaultExpression.GetInfo<TimeZone>();
            Console.WriteLine(text);

            // *---------------------- About generics --------------------- *//
            DefaultExpression listStringDefaultExpression = Expression.Default(typeof(List<string>));
            text = listStringDefaultExpression.GetInfo<List<string>>();
            Console.WriteLine(text);

            DefaultExpression listIntDefaultExpression = Expression.Default(typeof(List<int>));
            text = listIntDefaultExpression.GetInfo<List<int>>();
            Console.WriteLine(text);

            DefaultExpression hashSetIntDefaultExpression = Expression.Default(typeof(HashSet<int>));
            text = hashSetIntDefaultExpression.GetInfo<HashSet<int>>();
            Console.WriteLine(text);

            DefaultExpression keyValuePairFromIntToStringSetIntDefaultExpression = Expression.Default(typeof(KeyValuePair<int , string>));
            text = keyValuePairFromIntToStringSetIntDefaultExpression.GetInfo<KeyValuePair<int , string>>();
            Console.WriteLine(text);

            DefaultExpression dictionaryFromIntToStringSetIntDefaultExpression = Expression.Default(typeof(Dictionary<int,string>));
            text = dictionaryFromIntToStringSetIntDefaultExpression.GetInfo<Dictionary<int , string>>();
            Console.WriteLine(text);

            /// DON'T use non predefined type (including 
            /// + primitive type and 
            /// + defined in third package by Microsoft
            /// ) to create an expression with default value.
            //DefaultExpression userClassDefaultExpression = Expression.Default(typeof(User));
            //text = stringDefaultExpression.GetInfo<User>();
            //Console.WriteLine(text);

        }
```

where

`DefaultExpression.GetInfo` extension method is defined in `DefaultExpressionsExtensionMethods` static class

`DefaultExpression.GetInfo` extension method in `ExpressionsExtensionMethods.cs`

```
public static class DefaultExpressionsExtensionMethods
    {
        public static string GetInfo<TDefaultExpression>(
            this DefaultExpression defaultExpression
        )
        {
            var compiledDelegateFunction = Expression.Lambda<Func<TDefaultExpression>>(defaultExpression).Compile(); // TDefaultExpression MUST be primitive data, otherwise, it will throw an exception at runtime.
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.AppendFormat("{0}\n" , defaultExpression.ToString());
            stringBuilder.AppendFormat("{0}\n" , compiledDelegateFunction()); 
            return stringBuilder.ToString();
        }
    }
```

It will output following

```
default(Boolean)
False

default(Byte)
0

default(Int16)
0

default(Int32)
0

default(Int64)
0

default(UInt16)
0

default(UInt32)
0

default(UInt64)
0

default(Single)
0

default(Double)
0

default(Char)


default(String)


default(DateTime)
0001/1/1 上午 12:00:00

default(TimeSpan)
00:00:00

default(TimeZone)


default(List`1)


default(List`1)


default(HashSet`1)


default(KeyValuePair`2)
[0, ]

default(Dictionary`2)


```

## reference
### API docs
+ [`Expression.Variable Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.variable?view=net-8.0)
+ [`Expression.Parameter Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.parameter?view=net-8.0)
+ [`Expression.Constant Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.constant?view=net-8.0)
+ [`Expression.Assign(Expression, Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.assign?view=net-8.0)
+ [`Expression.Default(Type) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.default?view=net-8.0)
+ [`DefaultExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.defaultexpression?view=net-8.0)
