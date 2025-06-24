# CH5 -- operations about dictionary
## objectives
You will learn how to

+ add a key-value pair to dictionary
+ try to add a key-value pair to dictionary
+ remove a key-value pair to dictionary
+ clear all key-value pairs to dictionary
+ get value of dictionary with specified key 
+ try to get value of dictionary with specified key 

## CH5-1 -- add a key-value pair to dictionary
### `Add` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.Add("Sonoda","Umi");
```

+ will adds a key-value pair whose key is `Sonoda`and value is `Umi` if the `Sonoda` does NOT exist in keys of dictionary.

+ throw an exception at runtime.

### `Item[TKey]` property

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary["Sonoda"] = "Umi";
```

+ will adds a key-value pair whose key is `Sonoda`and value is `Umi` if the `Sonoda` does NOT exist in keys of dictionary.

+ throw an exception at runtime.

#### Remarks
> [!Important]
> Adding an existing key through `Add` instance method call will cause exception at runtime. 

## CH5.2 -- try to add a key-value pair to dictionary
### `TryAdd` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.TryAdd("Sonoda","Umi");
```

+ will adds a key-value pair whose key is `Sonoda`and value is `Umi` if the `Sonoda` does NOT exist in keys of dictionary. 

+ do nothing, otherwise.

## CH5.3 -- remove a key-value pair to dictionary
### `Remove` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.Remove("Sonoda","Umi");
```

+ will remove an entry whose key is `Sonoda` if `Sonoda` exists in keys of dictionary.

+ do nothing, otherwise.

## CH5.4 -- clear all key-value pairs to dictionary
### `Clear` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.Clear();
```

+ will remove all entries of dictionary.

## CH5.5 -- get value of dictionary with specified key 
### `Item[TKey]` property

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
string value = dictionary["Sonoda"];
```

+ will get corrsponding value with given key if the key exists.

+ it will cause exception at runtime if the given key does NOT exist.

## CH5.6 -- try to get value of dictionary with specified key
### `TryGetValue` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
key = "Ayase";
isValueExist = dictionary.TryGetValue(key , out value);
```

will try to get the corresponding value with given key.

+ if the `key` exists, then `value` will be set to corresponding value with given key and will return true

+ if the `key` does NOT exist, then `value` will be set to default value of `value` type and will return false.

## reference
### API docs
+ [`Dictionary<TKey,TValue>.Add(TKey, TValue) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.add?view=net-8.0)
