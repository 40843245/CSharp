# CH5 -- operations about dictionary
## objectives
You will learn how to

+ add a key-value pair to dictionary
+ try to add a key-value pair to dictionary
+ remove a key-value pair from dictionary
+ clear all key-value pairs of dictionary
+ get value from dictionary with specified key 
+ try to get value from dictionary with specified key 

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

## CH5.3 -- remove a key-value pair from dictionary
### `Remove` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.Remove("Sonoda","Umi");
```

+ will remove an entry whose key is `Sonoda` if `Sonoda` exists in keys of dictionary.

+ do nothing, otherwise.

## CH5.4 -- clear all key-value pairs of dictionary
### `Clear` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.Clear();
```

+ will remove all entries of dictionary.

## CH5.5 -- get value from dictionary with specified key 
### `Item[TKey]` property

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
string value = dictionary["Sonoda"];
```

+ will get corrsponding value with given key if the key exists.

+ it will cause exception at runtime if the given key does NOT exist.

## CH5.6 -- try to get value from dictionary with specified key
### `TryGetValue` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
key = "Ayase";
isValueExist = dictionary.TryGetValue(key , out value);
```

will try to get the corresponding value with given key.

+ if the `key` exists, then `value` will be set to corresponding value with given key and will return true

+ if the `key` does NOT exist, then `value` will be set to default value of `value` type and will return false.

## examples
### example 1
#### main code

```
        /// <summary>
        /// illustrate some commonly used operation in `Dictionary<TKey,TValue>` class.
        /// </summary>
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string key = string.Empty;
            string value;
            bool isValueExist = false;

            Dictionary<string , string> dictionary = new Dictionary<string , string>();
            dictionary.Add("Yazawa" , "Nico");

            Dictionary<string , string>.KeyCollection keys = dictionary.Keys;
            Dictionary<string , string>.ValueCollection values = dictionary.Values;

            Console.WriteLine(dictionary.GetInfo());

            dictionary.Add("Ayase" , "Eli");
            
            Console.WriteLine("After adding (\"Ayase\" \"Eli\") to dictionary,");

            Console.WriteLine(dictionary.GetInfo());

            dictionary.TryAdd("Yazawa" , "Nico");

            Console.WriteLine("After try to add (\"Yazawa\" \"Nico\") to dictionary,");

            Console.WriteLine(dictionary.GetInfo());

            dictionary.Remove("Ayase");
            
            Console.WriteLine("After removing (\"Ayase\" key) from dictionary,");

            Console.WriteLine(dictionary.GetInfo());

            key = "Yazawa";
            isValueExist = dictionary.TryGetValue(key,out value);
            if(isValueExist)
            {
                Console.WriteLine("The key `{0}` has value `{1}`" ,key, value);
            }
            else
            {
                Console.WriteLine("The key `{0}` does NOT exist",key);
            }

            key = "Ayase";
            isValueExist = dictionary.TryGetValue(key , out value);
            if(isValueExist)
            {
                Console.WriteLine("The key `{0}` has value `{1}`" , key , value);
            }
            else
            {
                Console.WriteLine("The key `{0}` does NOT exist" , key);
            }

            dictionary.Clear();

            Console.WriteLine("After clearing all key-value pairs of dictionary,");
            
            key = "Yazawa";
            isValueExist = dictionary.TryGetValue(key , out value);
            if(isValueExist)
            {
                Console.WriteLine("The key `{0}` has value `{1}`" , key , value);
            }
            else
            {
                Console.WriteLine("The key `{0}` does NOT exist" , key);
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod4 method call,
There are 1 elements in the dictionary
The key `Yazawa` has `Nico`.

After adding ("Ayase" "Eli") to dictionary,
There are 2 elements in the dictionary
The key `Yazawa` has `Nico`.
The key `Ayase` has `Eli`.

After try to add ("Yazawa" "Nico") to dictionary,
There are 2 elements in the dictionary
The key `Yazawa` has `Nico`.
The key `Ayase` has `Eli`.

After removing ("Ayase" key) from dictionary,
There are 1 elements in the dictionary
The key `Yazawa` has `Nico`.

The key `Yazawa` has value `Nico`
The key `Ayase` does NOT exist
After clearing all key-value pairs of dictionary,
The key `Yazawa` does NOT exist
End of TestMethod4 method call,
```

## reference
### API docs
+ [`Dictionary<TKey,TValue>.Add(TKey, TValue) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.add?view=net-8.0)
