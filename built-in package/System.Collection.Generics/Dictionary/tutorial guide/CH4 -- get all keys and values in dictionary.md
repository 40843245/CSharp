# CH4 -- get all keys and values in dictionary
## objectives
You will learn how to

+ get all keys in dictionary

+ get all values in dictionary

## CH4.1 -- get all keys in dictionary
### Keys getter property

```
public static void PrintDictionary<TKey,TValue>(
  Dictionary<TKey,TValue> dictionary
)
{
  Dictionary<TKey , TValue>.KeyCollection keys = dictionary.Keys;
  Dictionary<TKey , TValue>.ValueCollection values = dictionary.Values;
}
```

will get all keys and values of the dictionary.

#### Time Complexity
`O(1)`

#### Remark
> [!Important]
> The order of the keys in the `Dictionary<TKey,TValue>.KeyCollection` is unspecified,
>
> but it is the same order as the associated values in the `Dictionary<TKey,TValue>.ValueCollection` returned by the `Values` property.

> [!IMPORTANT]
> `Keys` property is not a static copy, thus it is reflected to keys of current dictionary.

## CH4.2 -- get all values in dictionary
### Values getter property

```
public static void PrintDictionary<TKey,TValue>(
  Dictionary<TKey,TValue> dictionary
)
{
  Dictionary<TKey , TValue>.KeyCollection keys = dictionary.Keys;
  Dictionary<TKey , TValue>.ValueCollection values = dictionary.Values;
}
```

will get all keys and values of the dictionary.

> [!IMPORTANT]
> `Values` property is not a static copy, thus it is reflected to values of current dictionary.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + get `Keys` and `Values` in a dictionary.
        /// 
        /// + also prove that the `Keys` and `Values` is reflected to dictionary.
        /// 
        /// That is, even one store `dictionary.Keys` to a variable, 
        /// 
        /// after `dictionary` changes, the variable will also changed.
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            
            string formattingString = "{0} {1}\n";
            List<string> textList = new List<string>();
            string text;

            Dictionary<string , string> dictionary = new Dictionary<string , string>();
            dictionary.Add("Yazawa","Nico");
            dictionary.Add("Ayase","Eli");
            dictionary.Add("Minami" , "Kotori");

            Dictionary<string , string>.KeyCollection keys = dictionary.Keys;
            Dictionary<string , string>.ValueCollection values = dictionary.Values;

            textList = keys.Apply(
                key => string.Format(formattingString , "First Name:" , key.ToString())
            );
            text = string.Join("", textList );
            Console.WriteLine(text);

            textList = values.Apply(
                value => string.Format(formattingString , "Last Name:" , value.ToString())
            );
            text = string.Join("", textList);
            Console.WriteLine(text);

            dictionary.Add("Sonoda","Umi");
            
            Console.WriteLine("After adding (\"Sonoda\" \"Umi\") to dictionary,");

            textList = keys.Apply(
                key => string.Format(formattingString , "First Name:" , key.ToString())
            );
            text = string.Join("" , textList);
            Console.WriteLine(text);

            textList = values.Apply(
                value => string.Format(formattingString , "Last Name:" , value.ToString())
            );
            text = string.Join("" , textList);
            Console.WriteLine(text);

            Console.WriteLine("End of {0} method call,", MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod3 method call,
First Name: Yazawa
First Name: Ayase
First Name: Minami

Last Name: Nico
Last Name: Eli
Last Name: Kotori

After adding ("Sonoda" "Umi") to dictionary,
First Name: Yazawa
First Name: Ayase
First Name: Minami
First Name: Sonoda

Last Name: Nico
Last Name: Eli
Last Name: Kotori
Last Name: Umi

End of TestMethod3 method call,
```

In the above example, we can prove that

the `Keys` and `Values` is reflected to keys and value of dictionary respectively.

## reference
### API docs
+ [`Dictionary<TKey,TValue>.Keys Property`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.keys?view=net-8.0)
+ [`Dictionary<TKey,TValue>.Values Property`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.values?view=net-8.0)
+ [`Dictionary<TKey,TValue>.KeyCollection Class`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.keycollection?view=net-8.0)
+ [`Dictionary<TKey,TValue>.ValueCollection Class`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.valuecollection?view=net-8.0)
