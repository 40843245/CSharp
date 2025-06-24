# CH8 -- count the number of entries of dictionary
## objectives
You will learn how to

+ count the number of entries of dictionary

## CH8.1 -- count the number of entries of dictionary
### `Count` getter property 

```
Dictionary<string , string> dictionaryWithCapacity = new Dictionary<string , string>(4);
dictionaryWithCapacity.Add("Yazawa" , "Nico");
dictionaryWithCapacity.Count; // 1
```

will have one entry.

## examples
### example 1
#### main code

Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + count the number of entries of dictionary.
        /// </summary>
        public static void TestMethod7()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            Dictionary<string , string> dictionaryWithCapacity = new Dictionary<string , string>(4);

            dictionaryWithCapacity.Add("Yazawa" , "Nico");
            dictionaryWithCapacity.Add("Ayase" , "Eli");
            dictionaryWithCapacity.Add("Minami" , "Kotori");

            Console.WriteLine("Current count:{0}" , dictionaryWithCapacity.Count);
            
            dictionaryWithCapacity.Add("Sonoda","Umi");

            Console.WriteLine("Current count:{0}" , dictionaryWithCapacity.Count);

            dictionaryWithCapacity.Add("Hoshizora","Rin");

            Console.WriteLine("Current count:{0}" , dictionaryWithCapacity.Count);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod7 method call,
Current count:3
Current count:4
Current count:5
End of TestMethod7 method call,
```

## reference
### API docs
+ [`Dictionary<TKey,TValue>.Count Property`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.count?view=net-8.0)
