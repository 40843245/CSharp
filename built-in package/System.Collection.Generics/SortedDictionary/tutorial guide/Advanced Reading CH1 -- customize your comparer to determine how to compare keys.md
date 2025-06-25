# Advanced Reading CH1 -- customize your comparer to determine how to compare keys
## objectives
You will learn how to

+ customize your comparer to determine how to compare keys

## Advanced Reading CH1.1 -- customize your comparer to determine how to compare keys
### `IComparer<TKey>`,

Step 1:

define a class that implements `IComparer<TKey>` interface and its methods.

you will need to implement these methods

1. `int Compare(TKey x , TKey y)`:

+ case 1:

if they are considered to be same,

return 0

+ case 2:

+ if `x` is considered to be greater than `y`,

return 1.

+ case 3:

+ if `x` is considered to be less than `y`,

return -1.

## examples
### example 1
#### customized comparer
In this example, the `NumericStringComparer` class will sort two strings 

using numeric comparison (if they can be converted to `int` or `double`)

Otherwise, use the alphebetically order (a default algorithm to sort two strings).

`NumericStringComparer.cs`

```
using System;
using System.Collections.Generic;

namespace Example.Utilities.Comparer
{
    /// <summary>
    /// compare two strings with numeric order.
    /// </summary>
    public class NumericStringComparer : IComparer<string>
    {
        public int Compare(string x , string y)
        {
            // Handle nulls first: null is considered less than any non-null string
            if(x == null && y == null)
            {
                return 0;
            }
            if(x == null)
            {
                return -1;
            }
            if(y == null)
            {
                return 1;
            }

            // Try to parse both strings as integers
            if(int.TryParse(x , out int numX) && int.TryParse(y , out int numY))
            {
                return numX.CompareTo(numY); // Compare as integers
            }

            // If one or both are not integers, try to parse as doubles (for floats/decimals)
            if(double.TryParse(x , out double dblX) && double.TryParse(y , out double dblY))
            {
                return dblX.CompareTo(dblY); // Compare as doubles
            }

            // Fallback: If neither can be parsed as a number, use standard string comparison.
            // StringComparison.Ordinal is generally fastest and good for culture-invariant comparisons.
            return String.Compare(x , y , StringComparison.Ordinal);
        }
    }
}
```

#### main code
Invoking following method 

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + define and use `IComparer<TKey>` to customize the sort of keys
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            SortedDictionary<string , string> sortedDictionaryWithIComparer = new SortedDictionary<string , string>(new NumericStringComparer());

            sortedDictionaryWithIComparer.Add("2" , "Eli");
            sortedDictionaryWithIComparer.Add("1" , "Nico");
            sortedDictionaryWithIComparer.Add("3" , "Kotori");
            sortedDictionaryWithIComparer.Add("5" , "Rin");
            sortedDictionaryWithIComparer.Add("4" , "Umi");

            Console.WriteLine(sortedDictionaryWithIComparer.GetInfo());

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod3 method call,
There are 5 elements in the sorted dictionary
The key `1` has `Nico`.
The key `2` has `Eli`.
The key `3` has `Kotori`.
The key `4` has `Umi`.
The key `5` has `Rin`.

End of TestMethod3 method call,
```

## reference
### API docs
+ [`IComparer<T> Interface`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1?view=net-9.0)
