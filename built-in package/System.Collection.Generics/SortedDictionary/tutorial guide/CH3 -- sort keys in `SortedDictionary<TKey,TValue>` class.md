# CH3 -- sort keys in `SortedDictionary<TKey,TValue>` class
## objectives
You will learn how to

+ sort keys in `SortedDictionary<TKey,TValue>` class

You will also know 

+ difference between `Dictionary<TKey,TValue>` class and `SortedDictionary<TKey,TValue>` class

## CH3.1 -- sort keys in `SortedDictionary<TKey,TValue>` class
One doesn't need to any thing.

When performing operation (such as adding new key-value pair, removing key-value pair),

it will sort the keys automatically.

## CH3.2 -- difference between `Dictionary<TKey,TValue>` class and `SortedDictionary<TKey,TValue>` class
As we discussed before,

> `SortedDictionary<TKey,TValue>` class will sort the keys automatically.
>
> while `Dictionary<TKey,TValue>` will NOT.

Let's prove it in example 1.

## examples
### example 1
### extension methods

1. About `Dictionary<TKey,TValue>`

`DictionaryExtensionMethods.cs`

```
using System.Text;

namespace Example.Extensions.ExtensionMethods.DictionaryExtensionMethods
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

2. About `SortedDictionary<TKey,TValue>`

`SortedDictionaryExtensionMethods.cs`

```
using System.Text;

namespace Example.Extensions.ExtensionMethods.SortedDictionaryExtensionMethods
{
    public static class SortedDictionaryExtensionMethods
    {
        public static string GetInfo<TKey,TValue>(
            this SortedDictionary<TKey,TValue> sortedDictionary    
        )
        where TKey: notnull
        {
            StringBuilder stringBuilder = new StringBuilder();

            if(sortedDictionary == null)
            {
                stringBuilder.AppendLine("The sorted dictionary is null");
                return stringBuilder.ToString();
            }

            if(sortedDictionary.Count <= 0)
            {
                stringBuilder.AppendLine("There are no elements in the sorted dictionary");
                return stringBuilder.ToString();
            }

            stringBuilder.AppendFormat("There are {0} elements in the sorted dictionary\n" , sortedDictionary.Count);
            foreach( var entry in sortedDictionary)
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
        /// prove that
        /// 
        /// + `SortedDictionary<TKey,TValue>` will sort the key
        /// 
        /// while `Dictionary<TKey,TValue>` will NOT.
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Dictionary<int , string> dictionary = new Dictionary<int , string>();
            SortedDictionary<int , string> sortedDictionary = new SortedDictionary<int , string>();

            dictionary.Add(2 , "Eli");
            dictionary.Add(1 , "Nico");
            dictionary.Add(3 , "Kotori");
            dictionary.Add(5 , "Rin");
            dictionary.Add(4 , "Umi");

            Console.WriteLine(dictionary.GetInfo());

            sortedDictionary.Add(2,"Eli");
            sortedDictionary.Add(1,"Nico");
            sortedDictionary.Add(3 , "Kotori");
            sortedDictionary.Add(5 , "Rin");
            sortedDictionary.Add(4 , "Umi");

            Console.WriteLine(sortedDictionary.GetInfo());

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod2 method call,
There are 5 elements in the dictionary
The key `2` has `Eli`.
The key `1` has `Nico`.
The key `3` has `Kotori`.
The key `5` has `Rin`.
The key `4` has `Umi`.

There are 5 elements in the sorted dictionary
The key `1` has `Nico`.
The key `2` has `Eli`.
The key `3` has `Kotori`.
The key `4` has `Umi`.
The key `5` has `Rin`.

End of TestMethod2 method call,
```

## reference
### API docs
+ [`SortedDictionary<TKey,TValue> Class`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2.-ctor?view=net-9.0)

+ [`SortedDictionary<TKey,TValue>.Add(TKey, TValue) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2.add?view=net-9.0)
