# Advance Reading CH3 -- Application of detect number of days given specified year
## objectives
You will know how to detect number of days given specified year through

defining a class that inherits `Calendar` abstract class,

implementing all its abstract method.

And write logic into your own method with 

`DaysInYearBeforeMinSupportedYear` getter property (in `Calendar` class).

## Application
### goal
Given a year `year`, 

get how many days there are in `year`.

### Use Case
Define a function f:R->R that given a year `year`, 

return how many days there are in `year`.

+ Use Case 1:

f(2006) = 365 

since year 2006 is NOT a leap year.

+ Use Case 1:

f(2000) = 366 

since year 2000 is a leap year.

### logic 
Given the year `year`,

you have to check the `year` is a leap year.

+ Case 1:

If `year` is a NOT leap year,

then it returns 365.

+ Case 2:

If `year` is a leap year,

then it returns 366.

### approach
#### 1th approach (NOT recommend)
To check `year` is a leap year, you can make it by 

yourself (such as write your own method)

For example,

```
public class DateHandler
{
    public bool IsLeapYear(int year)
    {
        if(year%400==0)
        {
            return true;
        }

        if(year%100==0)
        {
            return false;
        }

        if(year%4==0)
        {
            return true;
        }

        return false;
    }

    public int GetNumberOfDay(int year)
    {
        if(IsLeapYear(year))
        {
            return 366;
        }

        return 365;
    }
}
```

but it's so troublesome, why NOT build upon the work of others?

there is a an `IsLeapYear` abstract method in `Calendar` class to do such thing.

#### 2th approach
Let's reach the goal with `IsLeapYear` abstract method in `Calendar` class.

For example,

```
public class DateHandler
{
    public Calendar CurrentCalendar {get;}

    public int GetNumberOfDay(int year)
    {
        if(CurrentCalendar.IsLeapYear(year))
        {
            return 366;
        }

        return 365;
    }
}
```

or

```
public static class DateHandler
{
    public static int GetNumberOfDay(int year,Calendar calendar)
    {
        if(calendar.IsLeapYear(year))
        {
            return 366;
        }

        return 365;
    }
}
```

But they are still ugly.

In above code snippets, we use `365` NOT constants.

Though, we can define a constant in class,

it is redundant as there is a `DaysInYearBeforeMinSupportedYear` getter property (in `Calendar` class)

which defines it as `365` by default.

Let's dig it in next section.

#### 3th approach
In previous section, we have mentioned that

> there is a `DaysInYearBeforeMinSupportedYear` 
> 
> getter property (in `Calendar` class)
> 
> which defines it as `365` by default.

However, it is protected, thus we need to defined 

our own class that inherits `Calendar` abstract 

and implements all abstract method.

With `IsLeapYear` instance method and 

`DaysInYearBeforeMinSupportedYear` const which is 365 by default, 

we can simply write

```
public class CustomLeapYearCalendar : Calendar
{
    public required Calendar CurrentCalender { get; init; }

    public int GetNumberOfDays(int year)
    {
        bool isLeapYear = CurrentCalender.IsLeapYear(year);
        // By default, `DaysInYearBeforeMinSupportedYear` returns 365.
        int numberOfDays = DaysInYearBeforeMinSupportedYear;
        return (isLeapYear == true) ? numberOfDays + 1 : numberOfDays;
    }

    public int GetNumberOfDays(int year,int era)
    {
        bool isLeapYear = CurrentCalender.IsLeapYear(year , era);
        // By default, `DaysInYearBeforeMinSupportedYear` returns 365.
        int numberOfDays = DaysInYearBeforeMinSupportedYear;
        return (isLeapYear == true)? numberOfDays+1 : numberOfDays;
    }
    // ... implemented methods are omitted for clarity
}
```

see example 1 for full code.

## examples
### example 1
#### custom calendar class
`CustomLeapYearCalendar.cs`

```
using System;
using System.Globalization;

namespace Example.Utilities.Calendars
{
    public class CustomLeapYearCalendar : Calendar
    {
        public required Calendar CurrentCalender { get; init; }

        public int GetNumberOfDays(int year)
        {
            bool isLeapYear = CurrentCalender.IsLeapYear(year);
            //bool isLeapYear = IsLeapYear(year); // or you can use it since it is implemented.
            // By default, `DaysInYearBeforeMinSupportedYear` returns 365.
            int numberOfDays = DaysInYearBeforeMinSupportedYear;
            return (isLeapYear == true) ? numberOfDays + 1 : numberOfDays;
        }

        public int GetNumberOfDays(int year,int era)
        {
            
            bool isLeapYear = CurrentCalender.IsLeapYear(year , era);
            //bool isLeapYear = IsLeapYear(year , era); // or you can use it since it is implemented.
            // By default, `DaysInYearBeforeMinSupportedYear` returns 365.
            int numberOfDays = DaysInYearBeforeMinSupportedYear;
            return (isLeapYear == true)? numberOfDays+1 : numberOfDays;
        }

        public override int GetDaysInYear(int year , int era)
        {
            
            return CurrentCalender.GetDaysInYear(year , era);
        }

        public override int [ ] Eras => CurrentCalender.Eras;

        public override DateTime AddMonths(DateTime time , int months)
        {
            return CurrentCalender.AddMonths(time, months);
        }

        public override DateTime AddYears(DateTime time , int years)
        {
            return CurrentCalender.AddYears(time , years);
        }

        public override int GetDayOfMonth(DateTime time)
        {
            return CurrentCalender.GetDayOfMonth(time);
        }

        public override DayOfWeek GetDayOfWeek(DateTime time)
        {
            return CurrentCalender.GetDayOfWeek(time);
        }

        public override int GetDayOfYear(DateTime time)
        {
            return CurrentCalender.GetDayOfYear(time);
        }

        public override int GetDaysInMonth(int year , int month , int era)
        {
            return CurrentCalender.GetDaysInMonth(year, month,era);
        }

        public override int GetEra(DateTime time)
        {
            return CurrentCalender.GetEra(time);
        }

        public override int GetMonth(DateTime time)
        {
            return CurrentCalender.GetMonth(time);
        }

        public override int GetMonthsInYear(int year , int era)
        {
            return CurrentCalender.GetMonthsInYear(year,era);
        }

        public override int GetYear(DateTime time)
        {
            return CurrentCalender.GetYear(time);
        }

        public override bool IsLeapDay(int year , int month , int day , int era)
        {
            return CurrentCalender.IsLeapDay(year, month , day , era);
        }

        public override bool IsLeapMonth(int year , int month , int era)
        {
            return CurrentCalender.IsLeapMonth(year , month , era);
        }

        public override bool IsLeapYear(int year , int era)
        {
            return CurrentCalender.IsLeapYear(year , era);
        }

        public override DateTime ToDateTime(int year , int month , int day , int hour , int minute , int second , int millisecond , int era)
        {
            return CurrentCalender.ToDateTime(year , month, day, hour , minute , second , millisecond, era);
        }
    }
}
```

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to 
        /// 
        /// + inherits `Calendar` abstract class and implements its all abstract methods.
        /// 
        /// + use `DaysInYearBeforeMinSupportedYear` getter property which is protected.
        /// 
        /// (`protected virtual int DaysInYearBeforeMinSupportedYear { get; }`)
        /// </summary>
        public static void TestMethod9()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            CultureInfo currentCultureInfo = CultureInfo.CurrentCulture;
            Calendar currentCultureInfoCalendar = currentCultureInfo.Calendar;
            CustomLeapYearCalendar customLeapYearCalendar = new CustomLeapYearCalendar()
            { 
                CurrentCalender = currentCultureInfoCalendar 
            };

            int year = 1990;
            for(year = 1990; year < 2050; year++)
            {
                bool isLeapYear = customLeapYearCalendar.IsLeapYear(year);
                int numberOfDays = customLeapYearCalendar.GetNumberOfDays(year);
                Console.WriteLine("Is year `{0}` a leap year?`{1}`. There are {2} days." ,year, isLeapYear,numberOfDays);
            }
        }
```

will output following

```
In TestMethod9 method call,
Is year `1990` a leap year?`False`. There are 365 days.
Is year `1991` a leap year?`False`. There are 365 days.
Is year `1992` a leap year?`True`. There are 366 days.
Is year `1993` a leap year?`False`. There are 365 days.
Is year `1994` a leap year?`False`. There are 365 days.
Is year `1995` a leap year?`False`. There are 365 days.
Is year `1996` a leap year?`True`. There are 366 days.
Is year `1997` a leap year?`False`. There are 365 days.
Is year `1998` a leap year?`False`. There are 365 days.
Is year `1999` a leap year?`False`. There are 365 days.
Is year `2000` a leap year?`True`. There are 366 days.
Is year `2001` a leap year?`False`. There are 365 days.
Is year `2002` a leap year?`False`. There are 365 days.
Is year `2003` a leap year?`False`. There are 365 days.
Is year `2004` a leap year?`True`. There are 366 days.
Is year `2005` a leap year?`False`. There are 365 days.
Is year `2006` a leap year?`False`. There are 365 days.
Is year `2007` a leap year?`False`. There are 365 days.
Is year `2008` a leap year?`True`. There are 366 days.
Is year `2009` a leap year?`False`. There are 365 days.
Is year `2010` a leap year?`False`. There are 365 days.
Is year `2011` a leap year?`False`. There are 365 days.
Is year `2012` a leap year?`True`. There are 366 days.
Is year `2013` a leap year?`False`. There are 365 days.
Is year `2014` a leap year?`False`. There are 365 days.
Is year `2015` a leap year?`False`. There are 365 days.
Is year `2016` a leap year?`True`. There are 366 days.
Is year `2017` a leap year?`False`. There are 365 days.
Is year `2018` a leap year?`False`. There are 365 days.
Is year `2019` a leap year?`False`. There are 365 days.
Is year `2020` a leap year?`True`. There are 366 days.
Is year `2021` a leap year?`False`. There are 365 days.
Is year `2022` a leap year?`False`. There are 365 days.
Is year `2023` a leap year?`False`. There are 365 days.
Is year `2024` a leap year?`True`. There are 366 days.
Is year `2025` a leap year?`False`. There are 365 days.
Is year `2026` a leap year?`False`. There are 365 days.
Is year `2027` a leap year?`False`. There are 365 days.
Is year `2028` a leap year?`True`. There are 366 days.
Is year `2029` a leap year?`False`. There are 365 days.
Is year `2030` a leap year?`False`. There are 365 days.
Is year `2031` a leap year?`False`. There are 365 days.
Is year `2032` a leap year?`True`. There are 366 days.
Is year `2033` a leap year?`False`. There are 365 days.
Is year `2034` a leap year?`False`. There are 365 days.
Is year `2035` a leap year?`False`. There are 365 days.
Is year `2036` a leap year?`True`. There are 366 days.
Is year `2037` a leap year?`False`. There are 365 days.
Is year `2038` a leap year?`False`. There are 365 days.
Is year `2039` a leap year?`False`. There are 365 days.
Is year `2040` a leap year?`True`. There are 366 days.
Is year `2041` a leap year?`False`. There are 365 days.
Is year `2042` a leap year?`False`. There are 365 days.
Is year `2043` a leap year?`False`. There are 365 days.
Is year `2044` a leap year?`True`. There are 366 days.
Is year `2045` a leap year?`False`. There are 365 days.
Is year `2046` a leap year?`False`. There are 365 days.
Is year `2047` a leap year?`False`. There are 365 days.
Is year `2048` a leap year?`True`. There are 366 days.
Is year `2049` a leap year?`False`. There are 365 days.
```