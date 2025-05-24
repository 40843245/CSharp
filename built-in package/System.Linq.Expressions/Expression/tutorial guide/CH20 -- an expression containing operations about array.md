# CH20 -- an expression containing operations about array
## objectives
You will learn about

+ an expression containing operations about array

## CH20.1 -- an expression containing operations about array
### `Expression.ArrayIndex` static method
creates an Expression that represents applying an array index operator.

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

## reference
### API docs
+ [`Expression.ArrayIndex Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.arrayindex?view=net-8.0)