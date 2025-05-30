# CH29 -- get dimension of an array
## objectives
You will learn how to

+ get dimension of an array

## CH29.1 -- get dimension of an array
### `GetArrayRank` instance method
Gets the number of dimensions in an array.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get dimension of an array
        /// </summary>
        public static void TestMethod32()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            //int integer1 = 2;
            int [ ] int1DArray = new int [ ] { };
            int [ , ] int2DArray = new int [ , ] { };
            int [,,] int3DArray = new int [ ,, ]{ };
            int [ ,,,] int4DArray = new int [ ,,,]{ };
            int [ ,,,, ] int5DArray = new int [,,,,]{ };

            //Console.WriteLine("The {0} is {1} dimesional array" , "integer1" , integer1.GetType().GetArrayRank()); // will throw exception, since integer1 is NOT an array.
            Console.WriteLine("The {0} is {1} dimesional array" , "int1DArray" , int1DArray.GetType().GetArrayRank());
            Console.WriteLine("The {0} is {1} dimesional array" , "int2DArray" , int2DArray.GetType().GetArrayRank());
            Console.WriteLine("The {0} is {1} dimesional array" , "int3DArray" , int3DArray.GetType().GetArrayRank());
            Console.WriteLine("The {0} is {1} dimesional array" , "int4DArray" , int4DArray.GetType().GetArrayRank());
            Console.WriteLine("The {0} is {1} dimesional array" , "int5DArray" , int5DArray.GetType().GetArrayRank());
        }
```

will output following

```
In TestMethod32 method call
The int1DArray is 1 dimesional array
The int2DArray is 2 dimesional array
The int3DArray is 3 dimesional array
The int4DArray is 4 dimesional array
The int5DArray is 5 dimesional array
```

## reference
### API docs
+ [`Type.GetArrayRank Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getarrayrank?view=net-8.0)