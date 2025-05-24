# CH20 -- an expression about array
## objectives
You will learn about

+ an expression containing operations about array
+ an expression containing operations about creating new array

## CH20.1 -- an expression containing operations about array
### `Expression.ArrayIndex` static method
creates an Expression that represents applying an array index operator.

### `Expression.ArrayAccess` static method
creates an `IndexExpression` to access a one-dimesional or multidimensional array.

### `Expression.ArrayLength` static method
creates a `UnaryExpression` that represents an expression for obtaining the length of a one-dimensional array.

## CH20.2 -- an expression containing operations about creating new array
### `Expression.NewArrayInit` static method
creates a `NewArrayExpression` that represents creating a one-dimensional array and initializing it from a list of elements.

### `Expression.NewArrayBounds` static method
creates a `NewArrayExpression` that represents creating an array that has a specified rank.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.ArrayIndex`.
        /// </summary>
        public static void TestMethod28()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            // 1. Define the array:
            int [ ] myArray = { 10 , 20 , 30 , 40 , 50 };

            // 2. Create an Expression representing the array:
            //    (ConstantExpression for the array instance)
            Expression arrayExpression = Expression.Constant(myArray , typeof(int [ ]));

            // 3. Create an Expression representing the index:
            //    (ConstantExpression for the integer index)
            int index = 2; // We want to access myArray[2], which is 30
            Expression indexExpression = Expression.Constant(index , typeof(int));

            // 4. Use Expression.ArrayIndex to create an expression for array access:
            //    This represents the operation: myArray[index]
            BinaryExpression arrayAccessExpression = Expression.ArrayIndex(arrayExpression , indexExpression);

            // 5. Compile the expression into a delegate:
            //    The delegate will return an int (the value at the specified index).
            Func<int> getArrayElement = Expression.Lambda<Func<int>>(arrayAccessExpression).Compile();

            // 6. Execute the compiled delegate:
            int result = getArrayElement();

            Console.WriteLine($"The value at index {index} is: {result}");

        }
```

will output following

```
In TestMethod28 method call,
The value at index 2 is: 30
```

### example 2
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.ArrayIndex`.
        /// </summary>
        public static void TestMethod29()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            int [ ] myArray = { 10 , 20 , 30 , 40 , 50 };

            // Let's create an expression that takes an array and an index as parameters
            ParameterExpression paramArray = Expression.Parameter(typeof(int [ ]) , "array");
            ParameterExpression paramIndex = Expression.Parameter(typeof(int) , "idx");

            // Create the ArrayIndex expression using the parameters
            BinaryExpression dynamicArrayAccess = Expression.ArrayIndex(paramArray , paramIndex);

            // Create a lambda expression that takes an int[] and an int, and returns an int
            Expression<Func<int [ ] , int , int>> getDynamicArrayElement =
                Expression.Lambda<Func<int [ ] , int , int>>(
                    dynamicArrayAccess ,
                    paramArray ,
                    paramIndex
                );

            // Compile the dynamic expression
            Func<int [ ] , int , int> compiledDynamicAccessor = getDynamicArrayElement.Compile();

            // Use the compiled delegate with different inputs
            int [ ] anotherArray = { 1 , 2 , 3 , 4 , 5 , 6 , 7 };
            Console.WriteLine($"Accessing anotherArray[4]: {compiledDynamicAccessor(anotherArray , 4)}"); 
            Console.WriteLine($"Accessing myArray[0]: {compiledDynamicAccessor(myArray , 0)}");
        }
```

will output following

```
In TestMethod29 method call,
Accessing anotherArray[4]: 5
Accessing myArray[0]: 10
```

### example 3
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.ArrayAccess`.
        /// </summary>
        public static void TestMethod30()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // This parameter expression represents a variable that will hold the two-dimensional array.
            ParameterExpression arrayExpression = Expression.Parameter(typeof(int [ , ]) , "Array");

            // This parameter expression represents a first array index.
            ParameterExpression firstIndexExpression = Expression.Parameter(typeof(int) , "FirstIndex");
            // This parameter expression represents a second array index.
            ParameterExpression secondIndexExpression = Expression.Parameter(typeof(int) , "SecondIndex");

            // The list of indexes.
            List<Expression> indexes = new List<Expression> { firstIndexExpression , secondIndexExpression };

            // This parameter represents the value that will be added to a corresponding array element.
            ParameterExpression valueExpression = Expression.Parameter(typeof(int) , "Value");

            // This expression represents an access operation to a multidimensional array.
            // It can be used for assigning to, or reading from, an array element.
            Expression arrayAccessExpression = Expression.ArrayAccess(
                arrayExpression ,
                indexes
            );

            // This lambda expression assigns a value provided to it to a specified array element.
            // The array, the index of the array element, and the value to be added to the element
            // are parameters of the lambda expression.
            Expression<Func<int [ , ] , int , int , int , int>> lambdaExpression =
                Expression.Lambda<Func<int [ , ] , int , int , int , int>>(
                    Expression.Assign(arrayAccessExpression , Expression.Add(arrayAccessExpression , valueExpression)) ,
                    arrayExpression ,
                    firstIndexExpression ,
                    secondIndexExpression ,
                    valueExpression
            );

            // Print out expressions.
            Console.WriteLine("Array Access Expression:");
            Console.WriteLine(arrayAccessExpression.ToString());

            Console.WriteLine("Lambda Expression:");
            Console.WriteLine(lambdaExpression.ToString());

            Console.WriteLine("The result of executing the lambda expression:");

            // The following statement first creates an expression tree,
            // then compiles it, and then executes it.
            // Parameters passed to the Invoke method are passed to the lambda expression.
            int [ , ] sampleArray = 
                { 
                    {10,  20,   30},
                    {100, 200, 300}
                };
            Console.WriteLine(lambdaExpression.Compile().Invoke(sampleArray , 1 , 1 , 5));

        }
```

will output following

```
In TestMethod30 method call,
Array Access Expression:
Array[FirstIndex, SecondIndex]
Lambda Expression:
(Array, FirstIndex, SecondIndex, Value) => (Array[FirstIndex, SecondIndex] = (Array[FirstIndex, SecondIndex] + Value))
The result of executing the lambda expression:
205
```

### example 4
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.ArrayLength`
        /// </summary>
        public static void TestMethod31()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // 1. Define the array:
            int [ ] myArray = { 10 , 20 , 30 , 40 , 50 };

            // 2. Create an Expression representing the array:
            //    (ConstantExpression for the array instance)
            Expression arrayExpression = Expression.Constant(myArray , typeof(int [ ]));

            // 3. Use Expression.ArrayLength to create an expression for getting the array's length:
            //    This represents the operation: myArray.Length
            UnaryExpression arrayLengthExpression = Expression.ArrayLength(arrayExpression);

            // 4. Compile the expression into a delegate:
            //    The delegate will return an int (the length of the array).
            Func<int> getArrayLength = Expression.Lambda<Func<int>>(arrayLengthExpression).Compile();

            // 5. Execute the compiled delegate:
            int result = getArrayLength();

            Console.WriteLine($"The length of the array is: {result}");
        }
```

will output following

```
In TestMethod31 method call,
The length of the array is: 5
```

### example 5
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.NewArrayInit`.
        /// </summary>
        public static void TestMethod32()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            List<Expression> trees = new List<Expression>()
            { 
                  Expression.Constant("oak"),
                  Expression.Constant("fir"),
                  Expression.Constant("spruce"),
                  Expression.Constant("alder") 
            };

            // Create an expression tree that represents creating and
            // initializing a one-dimensional array of type string.
            NewArrayExpression newArrayExpression =
                Expression.NewArrayInit(typeof(string) , trees);

            // Output the string representation of the Expression.
            Console.WriteLine(newArrayExpression.ToString());
        }
```

will output following

```
In TestMethod32 method call,
new [] {"oak", "fir", "spruce", "alder"}
```

### example 6
Invoking following method

```
        /// <summary>
        /// illustrate how to use `Expression.NewArrayBounds`.
        /// </summary>
        public static void TestMethod33()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            // Create an expression tree that represents creating a
            // two-dimensional array of type string with bounds [3,2].
            NewArrayExpression newArrayExpression =
                Expression.NewArrayBounds(
                        typeof(string) ,
                        Expression.Constant(3) ,
                        Expression.Constant(2)
                );

            // Output the string representation of the Expression.
            Console.WriteLine(newArrayExpression.ToString());
        }
```

will output following

```
In TestMethod33 method call,
new System.String[,](3, 2)
```

## reference
### API docs
+ [`Expression.ArrayIndex Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.arrayindex?view=net-8.0)
+ [`Expression.ArrayLength(Expression) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.arraylength?view=net-8.0)
+ [`Expression.ArrayAccess Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.arrayaccess?view=net-8.0)
+ [`IndexExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.indexexpression?view=net-8.0)
+ [`Expression.NewArrayBounds Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.newarraybounds?view=net-8.0)
+ [`Expression.NewArrayInit Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.newarrayinit?view=net-8.0)
+ [`NewArrayExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.newarrayexpression?view=net-8.0)