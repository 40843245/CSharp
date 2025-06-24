# CH3 -- iterate all entries in dictionary
## objectives
You will learn how to

+ iterate all entries in dictionary

## CH3-1 -- iterate all entries in dictionary
Using foreach

```
public static void PrintDictionary<TKey,TValue>(
  Dictionary<TKey,TValue> dictionary
)
{
  foreach(KeyValuePair<TKey,TValue> entry in dictionary)
  {
    TKey key = entry.Key;
    TValue value = entry.Value;
  }
}
```

## examples
### example 1
#### extension methods
`GetInfo` extension methods is defined in `DictionaryExtensionMethods` static class.

`DictionaryExtensionMethods.cs`

```
using System.Text;

namespace Example.Extensions.ExtensionMethods
{
    public static class DictionaryExtensionMethods
    {
        public static string GetInfo<TKey,TValue>(
            this Dictionary<TKey,TValue> dictionary    
        )
        where TKey: notnull
        {
            StringBuilder stringBuilder = new StringBuilder();

            if(dictionary == null)
            {
                stringBuilder.AppendLine("The dictionary is null");
                return stringBuilder.ToString();
            }

            if(dictionary.Count <= 0)
            {
                stringBuilder.AppendLine("There are no elements in the dictionary");
                return stringBuilder.ToString();
            }

            stringBuilder.AppendFormat("There are {0} elements in the dictionary\n",dictionary.Count);
            foreach( var entry in dictionary)
            {
                string key = entry.Key.ToString();
                string value = entry.Value?.ToString();
                value = ( value != null ? $"`{value}`" : "null");
                stringBuilder.AppendFormat("The key `{0}` has {1}.\n", key, value);
            }
            return stringBuilder.ToString();
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
        /// + iterate all entries (`KeyValuePair<TKey,TValue>` type) in a dictionary. 
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            Dictionary<string , string> dictionary = new Dictionary<string , string>();
            Dictionary<string , string> otherDictionary = new Dictionary<string , string>(dictionary);

            otherDictionary.Add("Yazawa","Nico");

            int counter = 1;

            ///* ---- Example 1 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.Write(dictionary.GetInfo());
            counter++;

            ///* ---- Example 2 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.Write(otherDictionary.GetInfo());
            counter++;

            Console.WriteLine("End {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod2 method call,
///* ---- Example 1 ---- *///
There are no elements in the dictionary
///* ---- Example 2 ---- *///
There are 1 elements in the dictionary
The key `Yazawa` has `Nico`.
End TestMethod2 method call,
```
