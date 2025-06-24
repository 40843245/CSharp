# CH10 -- get comparer used by dictionary
## objectives
You will learn how to 

+ get comparer used by dictionary
  
## CH10.1 -- get comparer used by dictionary
### `Comparer` getter property

```
Dictionary<string, int> otherDictionaryWithIEqualityComparer = new Dictionary<string , int>(new NumericStringEqualityComparer());
otherDictionaryWithIEqualityComparer.Comparer;
```

will return currently comparer used by `otherDictionaryWithIEqualityComparer` instance.

Here, it is `NumericStringEqualityComparer`.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + get current equality comparer (return value is type of `IEqualityComparer<T>` or its dervied class)
        /// </summary>
        /// <remarks>
        /// If no equality comparer is specified, it will use default comparer --[EqualityComparer<T>.Default](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.equalitycomparer-1.default?view=net-8.0)
        /// </remarks>
        public static void TestMethod9()
        {

            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            Dictionary<string,string> dictionary = new Dictionary<string,string>();
            Dictionary<int , string> otherDictionary = new Dictionary<int , string>();
            Dictionary<string,string> dictionaryWithIEqualityComparer = new Dictionary<string , string>(new NumericStringEqualityComparer());
            Dictionary<string, int> otherDictionaryWithIEqualityComparer = new Dictionary<string , int>(new NumericStringEqualityComparer());

            int counter = 1;

            ///* ---- Example 1 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///",counter);
            Console.WriteLine("Equality comparer that currently used of instance named `dictionary` :{0}" , dictionary.Comparer);
            counter++;

            ///* ---- Example 2 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.WriteLine("Equality comparer that currently used of instance named  `otherDictionary`:{0}" , otherDictionary.Comparer); 
            counter++;

            ///* ---- Example 3 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.WriteLine("Equality comparer that currently used of instance named `dictionaryWithIEqualityComparer` :{0}" , dictionaryWithIEqualityComparer.Comparer);
            counter++;

            ///* ---- Example 4 ---- *///
            Console.WriteLine("///* ---- Example {0} ---- *///" , counter);
            Console.WriteLine("Equality comparer that currently used of instance named `otherDictionaryWithIEqualityComparer` :{0}" , otherDictionaryWithIEqualityComparer.Comparer);
            counter++;

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod9 method call,
///* ---- Example 1 ---- *///
Equality comparer that currently used of instance named `dictionary` :System.Collections.Generic.GenericEqualityComparer`1[System.String]
///* ---- Example 2 ---- *///
Equality comparer that currently used of instance named  `otherDictionary`:System.Collections.Generic.GenericEqualityComparer`1[System.Int32]
///* ---- Example 3 ---- *///
Equality comparer that currently used of instance named `dictionaryWithIEqualityComparer` :Example.Utilities.Comparer.NumericStringEqualityComparer
///* ---- Example 4 ---- *///
Equality comparer that currently used of instance named `otherDictionaryWithIEqualityComparer` :Example.Utilities.Comparer.NumericStringEqualityComparer
End of TestMethod9 method call,
```

## reference
### API docs
+ [`Dictionary<TKey,TValue>.Comparer Property`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.comparer?view=net-8.0)

+ [`IEqualityComparer<T> Interface`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.iequalitycomparer-1?view=net-8.0)
