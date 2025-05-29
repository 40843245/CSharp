# CH23 -- get element type
## objectives
You will learn how to

+ get element type

## CH23.1 -- get element type
### `GetElementType` instance method
see following example.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get element type.
        /// </summary>
        public static void TestMethod26()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            string elementTypeString = string.Empty;

            Type elementType;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            int [ ] intArray = new int [ ]
            {
                1,
                2,
                3,
            };

            Type intArrayType = intArray.GetType();

            elementType = intArrayType.GetElementType();
            elementTypeString = elementType?.FullName ?? "null";

            Console.WriteLine("The {0} type has element type {1}" , intArrayType, elementTypeString);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Volume volume = new Volume();

            Type volumeType = volume.GetType();

            elementType = volumeType.GetElementType();
            elementTypeString = elementType?.FullName ?? "null";

            Console.WriteLine("The {0} type has element type {1}" , volumeType , elementTypeString);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod26 method call
///* ------------------ Example 1 ------------------ *///
The System.Int32[] type has element type System.Int32

///* ------------------ Example 2 ------------------ *///
The Example.Bean.Volume type has element type null

```

## reference
### API docs
+ [`Type.GetElementType Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getelementtype?view=net-8.0)