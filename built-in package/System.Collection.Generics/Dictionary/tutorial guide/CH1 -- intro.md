# CH1 -- intro
## objectives
You will know

+ what is dictionary

+ which class can store and manipulate data

 ## CH1.1 -- What is dictionary
 A dictionary consists of key-value pair

Each key-value pair has a key and a vak=lue.

The key corresponding its value.

## CH1.2 -- which class can store and manipulate data
+ `Dictionary<TKey,Value>`
+ `SortedDictionary<Tkey,TValue>`

### Remark
> [!IMPORTANT]
> Note that 
>
> the definition of `Dictionary<TKey,Value>` class stores key-value pairs with hash code.
>
> Thus, it will NOT iterate entries by order.
>


For example,

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.Add("Yazawa","Nico");
dictionary.Add("Ayase","Eli");
dictionary.Add("Minami","Kotori");

foreach(KeyValuePair<string,string> entry in dictionary)
{
  Console.WriteLine("{0} {1}",entry.Key,entry.Value);
}
```

might not output following

```
Yazawa Nico
Ayase Eli
Minami Kotori
```

> [!IMPORTANT]
> If you want to sort key by order, consider use `SortedDictionary<TKey , TValue>`.

For example,

```
SortedDictionary<string , string> sortedDictionary = new SortedDictionary<string , string>();
sortedDictionary.Add("Yazawa","Nico");
sortedDictionary.Add("Ayase","Eli");
sortedDictionary.Add("Minami","Kotori");

foreach(KeyValuePair<string,string> entry in sortedDictionary)
{
  Console.WriteLine("{0} {1}",entry.Key,entry.Value);
}
```

will always output following

```
Ayase Eli
Minami Kotori
Yazawa Nico
```

since the `sortedDictionary` uses default comparer (here is `Comparer<string>.Default`) to sort keys.

> [!IMPORTANT]
> `Comparer<string>.Default` compares two strings through following:
> 
>  1. get the first char of strings, then compare its ascii code of two chars.
>  2. if two chars has same ascii code.
>  3. get the second char of strings, then compare its ascii code of two chars
>  4. and so on until one string is reached so that there are no available char to be read.

