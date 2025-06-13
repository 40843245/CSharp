# CH1 -- get default calender of current culture info
## objectives
You will learn how to

+ get default calender of current culture info

## CH1.1 -- get default calender of current culture info

```
CultureInfo cultureInfo = CultureInfo.CurrentCulture;
```

will return current culture info.

```
Calendar calendar = cultureInfo.Calendar;
```

will return the default calendar of `CultureInfo` -- `cultureInfo`.

Combining them together

```
CultureInfo cultureInfo = CultureInfo.CurrentCulture;
Calendar calendar = cultureInfo.Calendar;
```

will get default calender of current culture info.

## reference
### API docs
+ [`CultureInfo.CurrentCulture Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.currentculture?view=net-8.0)

+ [`CultureInfo.Calendar Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.calendar?view=net-8.0)

