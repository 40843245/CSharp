# CH9 -- manage capacity of dictionary
## objectives
You will learn how to manage capacity. Including

+ set the initial capacity
+ ensure the capacity
+ trim capacity

## CH9.1 -- set the initial capacity

```
/// instaniate a dictionary with intial capacity as 4
Dictionary<string , string> dictionaryWithCapacity = new Dictionary<string , string>(4);
```

will instaniate a dictionary with intial capacity as 4.

## CH9.2 -- ensure the capacity
### `EnsureCapacity` instance method (C# 5.0 or above)
To ensure capacity at least of given number after initialization, you can use `EnsureCapacity` instance method

```
Dictionary<string , string> dictionaryWithCapacity = new Dictionary<string , string>(4);
int ensuredCapacity = dictionaryWithCapacity.EnsureCapacity(5);
```

will ensure the capacity is at least 5. 

However, capacity is likely to be 7 even if the we ensure the capacity is at least 5 and there are no more than 5 entries.

since due to internal implementaion for better performance and maintain.

see [Google Gemini's response -- `C#` Dictionary Capacity Explained](https://g.co/gemini/share/e4b5d675176e)

## CH9.3 -- trim capacity
### `TrimExcess` instance method

If the dictionary is too large but only has fewer entries,

then we can reallocate it (trim unused memory) through `TrimExcesss` instance method.

+ `TrimExcess()`:
   
```
dictionaryWithCapacity.TrimExcess();
```

will trim the capacity down to the number of entries of dictionary as small as possible.

+ `TrimExcess()`:

```
dictionaryWithCapacity.TrimExcess(5);
```

will trim the capacity down to the number 5 as small as possible.

#### Remarks

> [!WARNING]
> In `TrimExcess(int capacity)` instance method,
>
> if you pass `capacity` which is less than current number of entries of dictionary,
>
> it will throw an exception `ArgumentOutOfRangeException` at runtime.

> [!IMPORTANT]
> `TrimExcess()` instance method will trim the capacity down to the number of entries of dictionary as small as possible,
>
> NOT trim the capacity to the number of entries of dictionary
>
> The reason why is about ue to internal implementation details, which expand the capacity if the capacity is too small, for better performance.
>
> it is likely expand the capacity to a prime number.
> 
> For more details, see [Google Gemini's response -- `C#` Dictionary Capacity Explained](https://g.co/gemini/share/e4b5d675176e)

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + manage capacity of dictionary.
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            int ensuredCapacity;
            /// instaniate a dictionary with intial capacity as 3
            Dictionary<string , string> dictionaryWithCapacity = new Dictionary<string , string>(3);

            dictionaryWithCapacity.Add("Yazawa" , "Nico");
            dictionaryWithCapacity.Add("Ayase" , "Eli");
            dictionaryWithCapacity.Add("Minami" , "Kotori");

            ensuredCapacity = dictionaryWithCapacity.EnsureCapacity(5);

            Console.WriteLine("The capacity is set to be a number at least {0}",ensuredCapacity);

            dictionaryWithCapacity.TrimExcess();

            dictionaryWithCapacity.Add("Sonoda" , "Umi");
            dictionaryWithCapacity.Add("Hoshizora" , "Rin");

            /// trim capacity to number of entries of `dictionaryWithCapacity`.
            /// ensure no more capacities are unused.
            dictionaryWithCapacity.TrimExcess(dictionaryWithCapacity.Count);

            dictionaryWithCapacity.Add("Nishikino" , "Maki");

            ensuredCapacity = dictionaryWithCapacity.EnsureCapacity(2);

            Console.WriteLine("The capacity is set to be a number at least {0}" , ensuredCapacity);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod8 method call,
The capacity is set to be a number at least 7
The capacity is set to be a number at least 7
End of TestMethod8 method call,
```

## reference
### API docs
+ [`Dictionary<TKey,TValue>(Int32)`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.-ctor?view=net-8.0#system-collections-generic-dictionary-2-ctor(system-int32))
+ [`Dictionary<TKey,TValue>.EnsureCapacity(Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.ensurecapacity?view=net-8.0)
+ [`Dictionary<TKey,TValue>.TrimExcess Method`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.trimexcess?view=net-8.0)

## further reading
+ About internal implementation, see [Google Gemini's response -- `C#` Dictionary Capacity Explained](https://g.co/gemini/share/e4b5d675176e)
