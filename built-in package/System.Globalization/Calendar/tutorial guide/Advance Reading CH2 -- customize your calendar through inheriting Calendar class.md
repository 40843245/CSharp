# Advance Reading CH2 -- customize your calendar through inheriting Calendar class
## objectives
You will 

+ learn how to Advance Reading CH2 -- customize your calendar through inheriting `Calendar` class

+ know which abstract that methods needs to be implemented

## Advance Reading CH2.1 -- abstract methods that needs to be implemented
These are abstract methods or properties that needs to be implemented.

```
public override int [ ] Eras => throw new NotImplementedException();
```

```
public override DateTime AddMonths(DateTime time , int months)
{
    throw new NotImplementedException();
}
```

```
public override DateTime AddYears(DateTime time , int years)
{
    throw new NotImplementedException();
}
```

```
public override int GetDayOfMonth(DateTime time)
{
    throw new NotImplementedException();
}
```

```
public override DayOfWeek GetDayOfWeek(DateTime time)
{
    throw new NotImplementedException();
}
```

```
public override int GetDayOfYear(DateTime time)
{
    throw new NotImplementedException();
}
```

```
public override int GetDaysInMonth(int year , int month , int era)
{
    throw new NotImplementedException();
}
```

```
public override int GetDaysInYear(int year , int era)
{
    throw new NotImplementedException();
}
```

```
public override int GetEra(DateTime time)
{
    throw new NotImplementedException();
}
```

```
public override int GetMonth(DateTime time)
{
    throw new NotImplementedException();
}
```

```
public override int GetMonthsInYear(int year , int era)
{
    throw new NotImplementedException();
}
```

```
public override int GetYear(DateTime time)
{
    throw new NotImplementedException();
}
```

```
public override bool IsLeapDay(int year , int month , int day , int era)
{
    throw new NotImplementedException();
}
```

```
public override bool IsLeapMonth(int year , int month , int era)
{
    throw new NotImplementedException();
}
```

```
public override bool IsLeapYear(int year , int era)
{
    throw new NotImplementedException();
}
```

```
public override DateTime ToDateTime(int year , int month , int day , int hour , int minute , int second , int millisecond , int era)
{
    throw new NotImplementedException();
}
```

## reference
### API docs
+ [`Calendar Class`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar?view=net-8.0)