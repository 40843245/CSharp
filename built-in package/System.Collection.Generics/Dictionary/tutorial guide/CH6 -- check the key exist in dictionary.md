# CH6 -- check the key exist in dictionary
## objectives
You will learn how to

+ check the key exist in dictionary

## CH6.1 -- check the key exist in dictionary
### `ContainsKey` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.ContainsKey(key);
```

will check the `key` is in the `dictionary`.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + check the dictionary contains specified key
        /// </summary>
        public static void TestMethod5()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            
            string key;

            Dictionary<string , string> dictionary = new Dictionary<string , string>();
            
            dictionary.Add("Yazawa" , "Nico");
            dictionary.Add("Ayase" , "Eli");
            dictionary.Add("Minami" , "Kotori");

            key = "Yazawa";
            if(dictionary.ContainsKey(key))
            {
                Console.WriteLine("The {0} key exists in the dictionary" , key);
            }
            else
            {
                Console.WriteLine("The {0} key deos NOT exist in the dictionary" , key);
            }

            key = "Ai";
            if(dictionary.ContainsKey(key))
            {
                Console.WriteLine("The {0} key exists in the dictionary" , key);
            }
            else
            {
                Console.WriteLine("The {0} key deos NOT exist in the dictionary" , key);
            }

            Console.WriteLine("End {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod5 method call,
The Yazawa key exists in the dictionary
The Ai key deos NOT exist in the dictionary
End TestMethod5 method call,
```

## reference
### API docs
+ [`Dictionary<TKey,TValue>.ContainsKey(TKey) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.containskey?view=net-8.0)
