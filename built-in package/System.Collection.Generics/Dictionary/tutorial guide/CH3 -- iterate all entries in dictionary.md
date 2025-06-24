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

