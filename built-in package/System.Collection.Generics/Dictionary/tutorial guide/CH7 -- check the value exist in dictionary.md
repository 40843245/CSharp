# CH7 -- check the value exist in dictionary
## objectives
You will learn how to

+ check the value exist in dictionary

## CH7.1 -- check the value exist in dictionary
### `ContainsValue` instance method

```
Dictionary<string , string> dictionary = new Dictionary<string , string>();
dictionary.ContainsValue(value);
```

will check the `value` is one of values in the `dictionary`.

## examples
### example 1
Invoking following method

```
                /// <summary>
        /// illustrate how to
        /// 
        /// + check the dictionary contains specified value
        /// </summary>
        public static void TestMethod6()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string value;

            Dictionary<string , string> dictionary = new Dictionary<string , string>();

            dictionary.Add("Yazawa" , "Nico");
            dictionary.Add("Ayase" , "Eli");
            dictionary.Add("Minami" , "Kotori");

            value = "Nico";
            if(dictionary.ContainsValue(value))
            {
                Console.WriteLine("The {0} value exists in the dictionary" , value);
            }
            else
            {
                Console.WriteLine("The {0} value deos NOT exist in the dictionary" , value);
            }

            value = "Ai";
            if(dictionary.ContainsValue(value))
            {
                Console.WriteLine("The {0} value exists in the dictionary" , value);
            }
            else
            {
                Console.WriteLine("The {0} value deos NOT exist in the dictionary" , value);
            }

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod6 method call,
The Nico value exists in the dictionary
The Ai value deos NOT exist in the dictionary
End of TestMethod6 method call,
```

## reference
### API docs
+ [`Dictionary<TKey,TValue>.ContainsValue(TValue) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.containsvalue?view=net-8.0)
