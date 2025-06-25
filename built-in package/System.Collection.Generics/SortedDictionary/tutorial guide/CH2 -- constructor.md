# CH2 -- constructor
## objectives
You will know

+ these constructors in `SortedDictionary<TKey,TValue>` class

## constructors in `SortedDictionary<TKey,TValue>` class

1. `SortedDictionary<TKey,TValue>()`:

```
SortedDictionary<string,string> sortedDictionary = new SortedDictionary<string,string>();
```

will instantiate a new empty sorted dictionary.

2. `SortedDictionary<TKey,TValue>(IDictionary<TKey,TValue>)`:

```
Dictionary<string,string> dictionary = new Dictionary<string,string>();
SortedDictionary<string,string> otherSortedDictionary = new SortedDictionary<string,string>(dictionary);
```

it will instantiate a new empty dictionary then

instantiate a dictionary that clones from the created dictionary.

3. `SortedDictionary<TKey,TValue>(IComparer<TKey>)`

```
SortedDictionary<string , string> sortedDictionaryWithIComparer = new SortedDictionary<string , string>(new NumericStringComparer()); 
```

it will instantiate a new empty sorted dictionary, using `new NumericStringComparer()` as the algorithm to sort keys. 

4. `SortedDictionary<TKey,TValue>(IDictionary<TKey,TValue>, IComparer<TKey>)`:

```
Dictionary<string,string> dictionary = new Dictionary<string,string>();
SortedDictionary<string , string> otherSortedDictionaryWithIComparer = new SortedDictionary<string , string>(dictionary,new NumericStringComparer());
```

it will instantiate a new empty dictionary then

instantiate a dictionary that clones from the created dictionary, using `new NumericStringComparer()` as the algorithm to sort keys. 

## reference
### API docs
+ [`SortedDictionary<TKey,TValue> Constructors`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2.-ctor?view=net-9.0)
