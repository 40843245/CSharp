# Advanced Reading CH1 -- get the number of days in the year
## objectives
You will learn how to

+ get the number of days in the year

## Advanced Reading CH1.1 -- get the number of days in the year
### `DaysInYearBeforeMinSupportedYear` getter property
It returns 365 by default.

However, it is marked as `protected` and `virtual`.

```
protected virtual int DaysInYearBeforeMinSupportedYear { get; }
```

So it can only used in a class that inherits `Calendar` class.

see example 1 in [Advanced Reading CH3](./Advanced%20Reading%20CH3%20--%20Application%20of%20detect%20number%20of%20days%20given%20specified%20year.md) for more understanding.

## reference
### API docs
+ [`Calendar.DaysInYearBeforeMinSupportedYear Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.daysinyearbeforeminsupportedyear?view=net-8.0)