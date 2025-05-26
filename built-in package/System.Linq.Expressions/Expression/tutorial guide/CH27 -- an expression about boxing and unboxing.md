# CH27 -- an expression about boxing and unboxing
## objective
You will learn

+ an expression about unboxing
+ an expression about boxing

## CH27.1 -- an expression about unboxing
### `Expression.Unbox` static method
To create an expression that used for `unboxing`, 

just simply invoking `Expression.Unbox` static method.

## examples
### example 1
The following example demonstrates

+ how to create an expression about `unboxing` -- `Expression.Unbox`
+ unbox value with same type as return type of the experssion with the expression

Invoking the following method

```
        /// <summary>
        /// illustrate how to create an expression about `unboxing` 
        /// 
        /// `Expression.Unbox`
        /// </summary>
        public static void TestMethod46()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            // 1. Define a parameter expression for an object (representing a boxed value type)
            ParameterExpression boxedValue = Expression.Parameter(typeof(object) , "boxedValue");

            // 2. Define the target value type (e.g., int)
            Type targetType = typeof(int);

            // 3. Create an Unbox expression
            // This expression will unbox 'boxedValue' to an 'int'
            Expression unboxExpression = Expression.Unbox(boxedValue , targetType);

            // 4. Create a Lambda Expression that takes the boxed object and returns the unboxed int
            // The lambda will look something like: (object boxedValue) => (int)boxedValue
            Expression<Func<object , int>> unboxLambda =
                Expression.Lambda<Func<object , int>>(unboxExpression , boxedValue);

            // 5. Compile the lambda expression into an executable delegate
            Func<object , int> unboxDelegate = unboxLambda.Compile();

            // 6. Test the delegate with a boxed integer
            int originalValue = 42;
            object boxedInteger = originalValue; // Implicit boxing

            int unboxedValue = unboxDelegate(boxedInteger);

            Console.WriteLine($"Original value: {originalValue}");
            Console.WriteLine($"Boxed value type: {boxedInteger.GetType()}");
            Console.WriteLine($"Unboxed value: {unboxedValue}");
            Console.WriteLine($"Unboxed value type: {unboxedValue.GetType()}");

            // Example with a different value type (double)
            ParameterExpression boxedDouble = Expression.Parameter(typeof(object) , "boxedDouble");
            Type targetDoubleType = typeof(double);
            Expression unboxDoubleExpression = Expression.Unbox(boxedDouble , targetDoubleType);
            Expression<Func<object , double>> unboxDoubleLambda =
                Expression.Lambda<Func<object , double>>(unboxDoubleExpression , boxedDouble);
            Func<object , double> unboxDoubleDelegate = unboxDoubleLambda.Compile();

            double originalDouble = 3.14;
            object boxedDoubleValue = originalDouble;
            double unboxedDouble = unboxDoubleDelegate(boxedDoubleValue);

            Console.WriteLine($"\nOriginal double: {originalDouble}");
            Console.WriteLine($"Boxed double type: {boxedDoubleValue.GetType()}");
            Console.WriteLine($"Unboxed double: {unboxedDouble}");
            Console.WriteLine($"Unboxed double type: {unboxedDouble.GetType()}");
        }
```

will output following

```
In TestMethod46 method call,
Original value: 42
Boxed value type: System.Int32
Unboxed value: 42
Unboxed value type: System.Int32

Original double: 3.14
Boxed double type: System.Double
Unboxed double: 3.14
Unboxed double type: System.Double
```

### example 2
The following example demonstrates

+ how to create an expression about `unboxing` -- `Expression.Unbox`
+ unbox value with wrong type as return type of the experssion with the expression and thus throw `System.ArgumentException`, but is catched with `catch` block.


Invoking following method

```
        /// <summary>
        /// illustrate how to create an expression about `unboxing` 
        /// 
        /// `Expression.Unbox`
        /// 
        /// But unbox the target value with wrong type.
        /// </summary>
        public static void TestMethod47()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // What happens if you try to unbox to the wrong type?
            // This will throw an InvalidCastException at runtime when the delegate is invoked.

            ParameterExpression boxedValue = Expression.Parameter(typeof(object) , "boxedValue");

            int originalValue = 42;
            object boxedInteger = originalValue; // Implicit boxing

            try
            {
                Type wrongTargetType = typeof(string);
                Expression wrongUnboxExpression = Expression.Unbox(boxedValue , wrongTargetType);
                Expression<Func<object , string>> wrongUnboxLambda =
                    Expression.Lambda<Func<object , string>>(wrongUnboxExpression , boxedValue);
                Func<object , string> wrongUnboxDelegate = wrongUnboxLambda.Compile();


                string wrongUnboxedValue = wrongUnboxDelegate(boxedInteger);
                Console.WriteLine($"Wrong unboxed value: {wrongUnboxedValue}");
            }
            catch(InvalidCastException ex)
            {
                Console.WriteLine($"Caught an `InvalidCastException` exception:\n {ex.Message}");
            }
            catch(ArgumentException ex)
            {
                Console.WriteLine($"Argument an `ArgumentException` exception:\n{ex.Message}");
            }
            finally
            {
                Console.WriteLine($"Finished to caught a boxed value: {boxedInteger}");
            }

        }
```

will output following

```
In TestMethod47 method call,
Caught an `ArgumentException` exception:
只能從物件或介面類型 Unbox 成為實值類型。
Finished to caught a boxed value: 42
```

## reference
### API docs
+ [`Expression.Unbox(Expression, Type) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.unbox?view=net-8.0)

### examples
+ The code in example 1 are generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/3d201e10b65f)

+ The code in example 1 are generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/3d201e10b65f) and modified by the author.