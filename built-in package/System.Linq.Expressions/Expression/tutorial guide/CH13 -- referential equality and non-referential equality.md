# CH13 -- referential equality and non-referential equality
## objectives
I will discussed about 

+ referential equality and non-referential equality between two expressions and delegates (after compiling)

## CH13.1 -- referential equality and non-referential equality between two expressions and delegates (after compiling)
> [!CAUTION]
> Watch out the referential equality and non-referential equality between two expressions, even if they have same behavior, and so for delegates that are compiled from expressions.

## examples
### example 1
Invoking this method

```
        /// <summary>
        /// Compare two delegate function are referentially equal.
        /// 
        /// First delegate function is directly initialized.
        /// 
        /// While, second delegate function is made from an expression in form of an expression tree.
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            ParameterExpression parameterExpression = Expression.Parameter(typeof(bool) , "result");

            Func<int , bool> delegateFunction1 = i => i < 5; // a delegate function.

            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.
            Func<int , bool> delegateFunction1FromExpression1 = expression1.Compile(); // compile it, making it as an delegate function.

            Func<int , bool> delegateFunction2 = i => i < 5; // a delegate function.

            Func<int , bool> delegateFunction2FromExpression1 = expression1.Compile(); // compile it, making it as an delegate function.

            Expression<Func<int , bool>> expression2 = i => i < 5; // represents a delegate function in form of an expression tree.
            Func<int , bool> delegateFunction1FromExpression2 = expression2.Compile(); // compile it, making it as an delegate function.

            Func<int , bool> delegateFunction2FromExpression2 = expression2.Compile(); // compile it, making it as an delegate function.

            int counter = 1;

            Console.WriteLine("In {0}th comparison" , counter);
            EqualMessageOutput.PrintInfo(
                delegateFunction1 ,
                delegateFunction2 ,
                "delegate function"
            );
            counter++;

            Console.WriteLine("In {0}th comparison",counter);
            EqualMessageOutput.PrintInfo(
                delegateFunction1 ,
                delegateFunction1FromExpression1 ,
                "delegate function"
            );
            counter++;

            Console.WriteLine("In {0}th comparison" , counter);
            EqualMessageOutput.PrintInfo(
                delegateFunction1FromExpression1 ,
                delegateFunction2FromExpression1 ,
                "delegate function"
            );
            counter++;

            List<BinaryExpression> binaryExpressionForReferenceEqual = new List<BinaryExpression>();

            List<BinaryExpression> binaryExpressionForReferenceNotEqual = new List<BinaryExpression>();

            binaryExpressionForReferenceEqual.Add(Expression.ReferenceEqual(expression1,expression2));

            binaryExpressionForReferenceNotEqual.Add(Expression.ReferenceNotEqual(expression1,expression2));

            Console.WriteLine("About `binaryExpressionForReferenceEqual`");
            binaryExpressionForReferenceEqual.ForEach(
                (binaryExpression) =>
                {
                    string infoText = binaryExpression.GetInfo();
                    Console.WriteLine(infoText);   
                }
            );
            Console.WriteLine();

            Console.WriteLine("About `binaryExpressionForReferenceNotEqual`");
            binaryExpressionForReferenceNotEqual.ForEach(
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

`EqualMessageOutput.PrintInfo` helper method is defined in `EqualMessageOutput` static class.

Defined as follows.

`EqualMessageOutput.cs`

```
        public static void PrintInfo(
            object object1, 
            object object2,
            string objectTypeName
        )
        {
            bool isEqual = object.Equals(object1 , object2);
            bool isReferentiallyEqual = object.ReferenceEquals(object1 , object2);
            Console.WriteLine($"Are these {objectTypeName}s equal? {isEqual}");

            Console.WriteLine($"Are these {objectTypeName}s referentially equal? {isReferentiallyEqual}");
        }
```

It will output following

```
In TestMethod2 method call,
In 1th comparison
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
In 2th comparison
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
In 3th comparison
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
About `binaryExpressionForReferenceEqual`
binaryExpression.Left:i => (i < 5)
binaryExpression.Right:i => (i < 5)
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False


About `binaryExpressionForReferenceNotEqual`
binaryExpression.Left:i => (i < 5)
binaryExpression.Right:i => (i < 5)
binaryExpression.IsLifted:False
binaryExpression.IsLiftedToNull:False


```

### example 2
Invoking following method

```
        /// <summary>
        /// Compare two delegate function are referentially equal.
        /// 
        /// First delegate function and second delegate function is made from an expression in form of an expression tree. But made with different `Compile` instance method call. 
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.

            Func<int , bool> delegateFunction1 = expression1.Compile(); // compile it, making it as an delegate function.
            Func<int , bool> delegateFunction2 = expression1.Compile(); // compile it, making it as an delegate function.

            EqualMessageOutput.PrintInfo(
                delegateFunction1 ,
                delegateFunction2 ,
                "delegate function"
            );
        }
```

where

`EqualMessageOutput.PrintInfo` static class is defined as above.

It will output following

```
In TestMethod3 method call,
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
```

### example 3
Invoking following method

```
        /// <summary>
        /// Compare two delegate function are referentially equal.
        /// 
        /// First delegate function and second delegate function is made from an expression in form of an expression tree. But made with different `Compile` instance method call. 
        /// </summary>
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.

            Func<int , bool> delegateFunction1 = expression1.Compile(false); // not compile it, using other way to make it as an delegate function.
            Func<int , bool> delegateFunction2 = expression1.Compile(false); // not compile it, using other way to make it as an delegate function.

            EqualMessageOutput.PrintInfo(
                delegateFunction1,
                delegateFunction2,
                "delegate function"
            );
        }
```

where

`EqualMessageOutput.PrintInfo` static class is defined as above.

It will output following

```
In TestMethod4 method call,
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
```

### example 4
Invoking following method

```
        /// <summary>
        /// referentially equal check
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.

            Func<int , bool> delegateFunction1 = expression1.Compile(false); // compile it, making it as an delegate function.
            Func<int , bool> delegateFunction2 = delegateFunction1; // points to first delegate function,

            EqualMessageOutput.PrintInfo(
                delegateFunction1 ,
                delegateFunction2 ,
                "delegate function"
            );
        }
```

where

`EqualMessageOutput.PrintInfo` static class is defined as above.

It will output following

```
In TestMethod5 method call,
Are these delegate functions equal? True
Are these delegate functions referentially equal? True
```

## reference
### API docs
+ [`Expression.ReferenceEqual(Expression, Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.referenceequal?view=net-8.0)
+ [`Expression.ReferenceNotEqual(Expression, Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.referencenotequal?view=net-8.0)
+ [`Object.Equals Method`](https://learn.microsoft.com/en-us/dotnet/api/system.object.equals?view=net-8.0)
+ [`Object.ReferenceEquals(Object, Object) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.object.referenceequals?view=net-8.0)
