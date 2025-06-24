# equality comparison in `C#`
##  `IEqualityComparer<T>` interface
### How it works
In `C#`, compare two instance are considered to same with `IEqualityComparer<T>`, it will do following step.

Step 1:

compare their hash code.

+ Case 1:

If their hash code are considered to be different 

(through compares the return values of `int GetHashCode([DisallowNull] T obj)` defined in customized equality comparer (class type) that implements  `IEqualityComparer<T>`),

then they can be considered to be different. exit.

+ Case 2:

If their hash code are considered to be same,

then go to Step 2.

Step 2:

It will invoke `Equals(T? x,T? y)` method to compare their value are considered to be same.

+ Case 1:

If it returns true, they are be considered to be same. exit.

+ Case 2:

If it returns false, they are be considered to be different. exit.

### Summary
In `C#`, compare two instance are considered to same with `IEqualityComparer<T>`, 

it will compare hash code first, 

then compare their value (if their hash code are considered to be same).

## reference
### Google Gemini
Thank's to Google Gemini. I've reviewed/learned the core concept of comparing two instance.

+ [How to compare equality (using `IEqualityComparer` interface) for `Dictionary<string,string>` which checks the two numeric string are equal (after converts it to number then compare it) in when try to add a key-value pair (through `TryAdd` method call) in `C#`?](https://github.com/40843245/CSharp/blob/main/C%23%20Tutorial/comparison/equality%20comparison/hash%20code.md)
