# Advanced Reading CH1 -- customize the behaviour of equality comparison for keys of dictionary
## objectives
You will learn how to

+ customize the behaviour of equality comparison for keys of dictionary

## Advanced Reading CH1.1 -- customize the behaviour of equality comparison for keys of dictionary
### `IEqualityComparer<T>` interface

Step 1:

Define a class that implements `IEqualityComparer<T>` interface and implements its all method.

Due to `IEqualityComparer<in T>` interface's definition as follows.

```
    public interface IEqualityComparer<in T>
    {
        bool Equals(T? x , T? y);
        int GetHashCode([DisallowNull] T obj);
    }
```

We need to implement these methods

+ `bool Equals(T? x , T? y);`
+ `int GetHashCode([DisallowNull] T obj);`

Step 2:

That's done. 

You can assign the `comparer` as instance that instiantiate from your own customized equality comparer.

## examples
### example 1
#### customized equality comparer
The `NumericStringEqualityComparer` will compare two double strings (a string containing a double) for keys of dictionary.

If two double strings can BOTH be converted into doubles, it will compare the two doubles are same.

For example,

"0","0.0","-0","-.0.0","0abc","0..abc" are BOTH to be considered same

The `NumericStringEqualityComparer` is defined as follows.

`NumericStringEqualityComparer.cs`

```
using System.Diagnostics.CodeAnalysis;
using System.Globalization;

namespace Example.Utilities.Comparer
{
    public class NumericStringEqualityComparer : IEqualityComparer<string>
    {

        // Helper method to extract the numeric prefix from a string
        // Returns true if a numeric part is found, and outputs the double value.
        // Returns false otherwise.
        private bool TryGetNumericPrefix(string? s , out double numericValue)
        {
            numericValue = 0.0;
            if(string.IsNullOrEmpty(s))
            {
                return false;
            }

            // Find the end of the numeric part
            int i = 0;
            // Handle optional leading sign
            if(s.Length > 0 && (s [ 0 ] == '-' || s [ 0 ] == '+'))
            {
                i++;
            }

            // Scan for digits and a single decimal point
            bool hasDecimal = false;
            while(i < s.Length)
            {
                if(char.IsDigit(s [ i ]))
                {
                    i++;
                }
                else if(s [ i ] == '.' && !hasDecimal)
                {
                    hasDecimal = true;
                    i++;
                }
                else
                {
                    break; // Non-numeric character found
                }
            }

            string numericPart = s.Substring(0 , i);

            // Use invariant culture for parsing to ensure consistent results
            if(double.TryParse(numericPart , NumberStyles.Float , CultureInfo.InvariantCulture , out numericValue))
            {
                return true;
            }

            return false;
        }

        public bool Equals(string? x , string? y)
        {
            if(ReferenceEquals(x , y)) return true; // Both null or same instance
            if(x == null || y == null) return false; // One is null, the other isn't

            // Try to extract numeric prefix from both
            bool xHasNumeric = TryGetNumericPrefix(x , out double numX);
            bool yHasNumeric = TryGetNumericPrefix(y , out double numY);

            if(xHasNumeric && yHasNumeric)
            {
                // Both have numeric prefixes, compare them as doubles
                return numX.Equals(numY);
            }
            else if(!xHasNumeric && !yHasNumeric)
            {
                // Neither has a numeric prefix, fall back to ordinal string comparison
                return String.Equals(x , y , StringComparison.Ordinal);
            }
            // One has a numeric prefix, the other doesn't. They are not equal.
            return false;
        }

        public int GetHashCode([DisallowNull] string obj)
        {

            if(obj == null)
            {
                // Following IEqualityComparer contract, if Equals(null, null) is true,
                // then GetHashCode(null) should be consistent.
                // However, GetHashCode is marked [DisallowNull], so obj should not be null.
                // If it somehow is, this will prevent NullReferenceException.
                return 0; // A common hash code for null or non-existent values
            }

            // Get the numeric prefix for hashing
            if(TryGetNumericPrefix(obj , out double numericValue))
            {
                // Hash based on the numeric value
                // Using a consistent hash for doubles is important
                // double.GetHashCode() is generally good enough
                return numericValue.GetHashCode();
            }
            
            // Fallback: Use standard string hash code for non-numeric strings
            return StringComparer.Ordinal.GetHashCode(obj);
        }
    }
}
```
#### main code

Invoking following method

```
        /// <summary>
        /// illustrate 
        /// 
        /// + the default behaviour of default equality comparer (`EqualityComparer<T>.Default`).
        /// 
        /// + how to customize the equality comparer (to compare two keys are same)
        /// 
        /// through defining a class that implements `IEqualityComparer<T>` and implements its all method.  
        /// </summary>
        public static void TestMethod10()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Dictionary<string , string> dictionary = new Dictionary<string , string>();
            Dictionary<string , string> dictionaryWithIEqualityComparer = new Dictionary<string , string>(new NumericStringEqualityComparer());

            string keyString;

            bool isKeyExist = false;

            int counter = 1;

            ///* ---- Example 1 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.WriteLine("///* ---- About operation of `dictionary` ---- *///");

            keyString = "-2";
            isKeyExist = dictionary.TryAdd(keyString , "-5");
            if(isKeyExist) 
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully",keyString,keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "2abc";
            isKeyExist =  dictionary.TryAdd(keyString , "5abc");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            Console.WriteLine(dictionary.GetInfo());

            counter++;

            ///* ---- Example 2 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.WriteLine("///* ---- About operation of `dictionaryWithIEqualityComparer` ---- *///");

            keyString = "2";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "2";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "2abc";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "2.0";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "2.0abc";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "2.0abc";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "0";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "0.0";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "-0";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "-0cb23";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "0.bc";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            keyString = "0..1abc";
            isKeyExist = dictionaryWithIEqualityComparer.TryAdd(keyString , "5");
            if(isKeyExist)
            {
                Console.WriteLine("The key `{0}` with type `{1}` does NOT exist and adding to dictionary successfully" , keyString , keyString.GetType());
            }
            else
            {
                Console.WriteLine("The key `{0}` with type `{1}` does exists and thus cause to add to dictionary with failure" , keyString , keyString.GetType());
            }

            Console.WriteLine(dictionaryWithIEqualityComparer.GetInfo());

            counter++;
            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod10 method call,
///* ---- Example 1 ---- *///
///* ---- About operation of `dictionary` ---- *///
The key `-2` with type `System.String` does NOT exist and adding to dictionary successfully
The key `2abc` with type `System.String` does NOT exist and adding to dictionary successfully
There are 2 elements in the dictionary
The key `-2` has `-5`.
The key `2abc` has `5abc`.

///* ---- Example 2 ---- *///
///* ---- About operation of `dictionaryWithIEqualityComparer` ---- *///
The key `2` with type `System.String` does NOT exist and adding to dictionary successfully
The key `2` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `2abc` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `2.0` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `2.0abc` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `2.0abc` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `0` with type `System.String` does NOT exist and adding to dictionary successfully
The key `0.0` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `-0` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `-0cb23` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `0.bc` with type `System.String` does exists and thus cause to add to dictionary with failure
The key `0..1abc` with type `System.String` does exists and thus cause to add to dictionary with failure
There are 2 elements in the dictionary
The key `2` has `5`.
The key `0` has `5`.

End of TestMethod10 method call,
```

## reference
### API docs
+ [`IEqualityComparer<T> Interface`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.iequalitycomparer-1?view=net-8.0)

### code
About the code snippet of example 1,

thanks to Google Gemini, it fixes the code snippet of customized equality comparer -- `NumericStringEqualityComparer` class 

(I got stuck it for a while, but I've review/learn lots of core concepts of equality comparison)

For explanation, see [Google Gemini's response -- Dictionary Key Behavior Explained](https://g.co/gemini/share/229cce242948)

## further reading
### Google Gemini
+ [Google Gemini's response -- Dictionary Key Behavior Explained](https://g.co/gemini/share/229cce242948)

### hash code 
