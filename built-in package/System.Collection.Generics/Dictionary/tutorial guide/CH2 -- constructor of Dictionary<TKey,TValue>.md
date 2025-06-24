# CH2 -- constructor of `Dictionary<TKey,TValue>`
## objectives
You will learn how

+ instanitate and instances with constructor

## 2.1 -- constructor of `Dictionary<TKey,TValue>`

```
    Dictionary<string , string> dictionary = new Dictionary<string , string>();
```

will instaniate an empty dictionary (i.e. no entries) using default `IEqualComparer` (used to compare the key is equal to others) 

```
    Dictionary<string , string> otherDictionary = new Dictionary<string , string>(dictionary);
```

will instaniate an dictionary with memberwise-cloned `dictionary` using default `IEqualComparer` (used to compare the key is equal to others) 

```
    Dictionary<string , string> dictionaryWithIEqualityComparer = new Dictionary<string , string>(new NumericStringEqualityComparer());
```

will instaniate an empty dictionary (i.e. no entries) using `NumericStringEqualityComparer()` that implements `IEqualComparer` (used to compare the key is equal to others) 

```
 Dictionary<string , string> otherDictionaryWithIEqualityComparer = new Dictionary<string , string>(dictionary , new NumericStringEqualityComparer());
```

will instaniate an dictionary with memberwise-cloned `dictionary` using `NumericStringEqualityComparer()` that implements `IEqualComparer` (used to compare the key is equal to others) 
