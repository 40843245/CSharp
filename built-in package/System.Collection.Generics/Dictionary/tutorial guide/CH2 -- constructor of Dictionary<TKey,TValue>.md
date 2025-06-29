# CH2 -- constructor of `Dictionary<TKey,TValue>`
## objectives
You will learn how to

+ instanitate and instances with constructor

## 2.1 -- constructor of `Dictionary<TKey,TValue>`
### overloads
+ `Dictionary<TKey,TValue>()`

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
```
  
will instaniate an empty dictionary (i.e. no entries) using default `IEqualComparer` (used to compare the key is equal to others) 

+ `Dictionary<TKey,TValue>(IDictionary<TKey,TValue>)`

```
Dictionary<string , string> otherDictionary = new Dictionary<string , string>(dictionary);
```

will instaniate an dictionary with memberwise-cloned `dictionary` using default `IEqualComparer` (used to compare the key is equal to others) 

+ `Dictionary<TKey,TValue>(IEqualityComparer<TKey>)`

```
Dictionary<string , string> dictionaryWithIEqualityComparer = new Dictionary<string , string>(new NumericStringEqualityComparer());
```

will instaniate an empty dictionary (i.e. no entries) using `NumericStringEqualityComparer()` that implements `IEqualComparer` (used to compare the key is equal to others) 

+ `Dictionary<TKey,TValue>(IDictionary<TKey,TValue>, IEqualityComparer<TKey>)`

```
Dictionary<string , string> otherDictionaryWithIEqualityComparer = new Dictionary<string , string>(dictionary , new NumericStringEqualityComparer());
```

will instaniate an dictionary with memberwise-cloned `dictionary` using `NumericStringEqualityComparer()` that implements `IEqualComparer` (used to compare the key is equal to others) 

+ `Dictionary<TKey,TValue>(Int32)`

```
Dictionary<string,string> dictionaryWithCapacity = new Dictionary<string , string>(4);
```

will instaniate an empty dictionary using default `IEqualComparer` (used to compare the key is equal to others) with inital capacity as 4.

## reference
### API docs
+ [Dictionary<TKey,TValue> Constructors](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.-ctor?view=net-8.0)
