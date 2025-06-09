# Memory Allocation
## `new`
For example

```
int[] numbers = new int[10];
for(var i=0;i<numbers.Length;i++){
  numbers[i] = i;
}
```

## `stackalloc`
For example, 

```
int length = 3;
Span<int> numbers = stackalloc int[length];
for (var i = 0; i < length; i++)
{
    numbers[i] = i;
}
```

```
unsafe
{
    int length = 3;
    int* numbers = stackalloc int[length];
    for (var i = 0; i < length; i++)
    {
        numbers[i] = i;
    }
}
```

## difference
See [Google Gemini's Answer](https://g.co/gemini/share/67141d4bb622)

## When to use which
See [Google Gemini's Answer](https://g.co/gemini/share/67141d4bb622)
