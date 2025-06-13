# CH2 -- get info from DateTime with calendar
## objectives
You will learn

+ with given `DateTime`, get info using some methods of `Calendar`

## CH2.1 -- with given `DateTime`, get info using some methods of `Calendar`
### `GetEra` instance method

```
int currentEra = calendar.GetEra(dateTime);
```

will return era (century) of `dateTime`.

### `GetYear` instance method

```
int currentYear = calendar.GetYear(dateTime);
```

will return year of `dateTime`.

### `GetMonth` instance method

```
int currentMonth = calendar.GetMonth(dateTime);
```

will return month of `dateTime`.

### `GetWeekOfYear` instance method

```
CalendarWeekRule calendarWeekRule = CalendarWeekRule
int currentWeekOfYear = calendar.GetWeekOfYear(dateTime,calendarWeekRule,dayOfWeek);
```

will return month of `dateTime`.

### `GetDayOfYear` instance method

```
int currentDayOfYear = calendar.GetDayOfYear(dateTime);
```

will return day of year of `dateTime`.

### `GetDayOfMonth` instance method

```
int currentDayOfMonth = calendar.GetDayOfMonth(dateTime);
```

will return day of month of `dateTime`.

### `GetDayOfWeek` instance method

```
int currentDayOfWeek = calendar.GetDayOfWeek(dateTime);
```

will return day of week of `dateTime`.

### `GetDaysInYear` instance method

```
int currentYear = calendar.GetYear(dateTime);
int currentDaysInYear = calendar.GetDaysInYear(currentYear);
```

will return the number of days in the specified year `currentYear`.

### `GetDaysInMonth` instance method

```
int currentYear = calendar.GetYear(dateTime);
int currentMonth = calendar.GetMonth(dateTime);
int currentDaysInMonth = calendar.GetDaysInMonth(currentYear,currentMonth);
```

will return the number of days in the specified year `currentYear` and month `currentMonth`.

### `GetHour` instance method

```
int currentHour = calendar.GetHour(dateTime);
```

will return hour of `dateTime`.

### `GetMinute` instance method

```
int currentMinute = calendar.GetMinute(dateTime);
```

will return minute of `dateTime`.

### `GetSecond` instance method

```
int currentSecond = calendar.GetSecond(dateTime);
```

will return second of `dateTime`.

### `GetMilliseconds` instance method

```
double currentMillisecond = calendar.GetMilliseconds(dateTime);
```

will return the millisecond of `dateTime`.

### `IsLeapYear` instance method

```
int currentYear = calendar.GetYear(dateTime);
bool isLeapYear = calendar.IsLeapYear(currentYear);
```

will return true iff specified year `currentYear` is a leap year.

### `IsLeapMonth` instance method

```
int currentYear = calendar.GetYear(dateTime);
int currentMonth = calendar.GetMonth(dateTime);
bool isLeapMonth = calendar.IsLeapMonth(currentYear, currentMonth);
```

will return true iff specified year `currentYear` and specified month `currentMonth`is a leap month.

### `IsLeapDay` instance method

```
int currentYear = calendar.GetYear(dateTime);
int currentMonth = calendar.GetMonth(dateTime);
int currentDaysInYear = calendar.GetDaysInYear(currentYear);
bool isLeapDay = calendar.IsLeapDay(currentYear , currentMonth , currentDaysInMonth);
```

will return true iff specified year `currentYear`, specified month `currentMonth`, specified day `currentDaysInYear` is a leap day.

## examples
### example 1
#### helper class
The following helper class helps us get a string 

representing the info of specified `DateTime` using some metod of `Calendar` instance.

`DateTimeInfoHelper.cs`

```
using Example.Constants;
using System;
using System.Globalization;
using System.Text;

namespace Example.Helper.DateTimeInfo
{
    public static class DateTimeInfoHelper
    {
        public static string GetInfo(
            Calendar calendar,
            CalendarWeekRule calendarWeekRule,
            DayOfWeek dayOfWeek,
            DateTime dateTime
        )
        {
            StringBuilder stringBuilder = new StringBuilder();;

            string customDateTimeFormat = DateTimeConstants.DateTimeFormatConstants.CustomDateTimeFormatConstants.ANNO_DOMINI_WITH_TIMEZONE_OFFSET_FORMAT;
            int currentEra;
            int currentYear;
            int currentMonth;
            int currentWeekOfYear;
            int currentDayOfYear;
            int currentDayOfMonth;
            int currentDaysInYear;
            int currentDaysInMonth;
            int currentHour;
            int currentMinute;
            int currentSecond;
            
            double currentMillisecond;

            DayOfWeek currentDayOfWeek;

            bool isLeapYear;
            bool isLeapMonth;
            bool isLeapDay;

            bool isReadOnly;

            CalendarAlgorithmType currentCalendarAlgorithmType;

            currentEra = calendar.GetEra(dateTime);
            currentYear = calendar.GetYear(dateTime);
            currentMonth = calendar.GetMonth(dateTime);
            currentWeekOfYear = calendar.GetWeekOfYear(dateTime,calendarWeekRule,dayOfWeek);
            currentDayOfYear = calendar.GetDayOfYear(dateTime);
            currentDayOfMonth = calendar.GetDayOfMonth(dateTime);
            currentDayOfWeek = calendar.GetDayOfWeek(dateTime);
            currentDaysInYear = calendar.GetDaysInYear(currentYear);
            currentDaysInMonth = calendar.GetDaysInMonth(currentYear,currentMonth);
            currentHour = calendar.GetHour(dateTime);
            currentMinute = calendar.GetMinute(dateTime);
            currentSecond = calendar.GetSecond(dateTime);
            currentMillisecond = calendar.GetMilliseconds(dateTime);

            isLeapYear = calendar.IsLeapYear(currentYear);
            isLeapMonth = calendar.IsLeapMonth(currentYear, currentMonth);
            isLeapDay = calendar.IsLeapDay(currentYear , currentMonth , currentDaysInMonth);

            currentCalendarAlgorithmType = calendar.AlgorithmType;

            isReadOnly = calendar.IsReadOnly;

            stringBuilder.AppendFormat("Is the DateTime `{0}` read only?`{1}`\n",dateTime.ToString(customDateTimeFormat),isReadOnly);
            stringBuilder.AppendFormat("Current Era:{0}\n" , currentEra);
            stringBuilder.AppendFormat("Current Year:{0}\n" , currentYear);
            stringBuilder.AppendFormat("Current Month:{0}\n" , currentMonth);
            stringBuilder.AppendFormat("Current Week Of Year:{0}\n" , currentWeekOfYear);
            stringBuilder.AppendFormat("Current Day Of Year:{0}\n" , currentDayOfYear);
            stringBuilder.AppendFormat("Current Day Of Month:{0}\n" , currentDayOfMonth);
            stringBuilder.AppendFormat("Current Day Of Week:{0}\n" , currentDayOfWeek);
            stringBuilder.AppendFormat("Current Days In Year:{0}\n" , currentDaysInYear);
            stringBuilder.AppendFormat("Current Days In Month:{0}\n" , currentDaysInMonth);
            stringBuilder.AppendFormat("Current Hour:{0}\n" , currentHour);
            stringBuilder.AppendFormat("Current Minute:{0}\n" , currentMinute);
            stringBuilder.AppendFormat("Current Second:{0}\n" , currentSecond);
            stringBuilder.AppendFormat("Current Millisecond:{0}\n" , currentMillisecond);
            stringBuilder.AppendFormat("Current CalendarAlgorithmType:{0}\n" , currentCalendarAlgorithmType);
            stringBuilder.AppendFormat("Is year {0} a leap year?`{1}`\n" , currentYear,isLeapYear);
            stringBuilder.AppendFormat("Is month {1} of year {0} a leap month?`{2}`\n" , currentYear,currentMonth,isLeapMonth);
            stringBuilder.AppendFormat("Is {2}th day of month {1} of year {0} a leap month?`{3}`\n" , currentYear,currentMonth,currentDaysInMonth,isLeapDay);

            return stringBuilder.ToString();
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
/// + get the info of specified `DateTime` using some metod of `Calendar` instance 
/// </summary>
public static void TestMethod1()
{
    Console.WriteLine("In {0} method call," ,MethodBase.GetCurrentMethod().Name);

    string infoText = string.Empty;

    CultureInfo cultureInfo = CultureInfo.CurrentCulture;
    Calendar calendar = cultureInfo.Calendar;
    DateTime dateTime;

    CalendarWeekRule calendarWeekRule;
    DayOfWeek dayOfWeek;

    int counter = 1;

    ///* --------- Example 1 --------- *///
    Console.WriteLine("///* --------- Example {0} --------- *///" , counter);

    calendarWeekRule = CalendarWeekRule.FirstFullWeek;
    dayOfWeek = DayOfWeek.Monday;

    // current `DateTime`
    dateTime = DateTime.Now;

    infoText = DateTimeInfoHelper.GetInfo(
        calendar,
        calendarWeekRule,
        dayOfWeek,
        dateTime
    );

    Console.WriteLine( infoText );
    counter++;

    ///* --------- Example 2 --------- *///
    Console.WriteLine("///* --------- Example {0} --------- *///" , counter);

    calendarWeekRule = CalendarWeekRule.FirstFullWeek;
    dayOfWeek = DayOfWeek.Monday;

    // 2005/01/31 00:00:00
    dateTime = new DateTime(2005,01,31,calendar);

    infoText = DateTimeInfoHelper.GetInfo(
        calendar ,
        calendarWeekRule ,
        dayOfWeek ,
        dateTime
    );

    Console.WriteLine(infoText);
    counter++;

    ///* --------- Example 3 --------- *///
    Console.WriteLine("///* --------- Example {0} --------- *///" , counter);

    calendarWeekRule = CalendarWeekRule.FirstFullWeek;
    dayOfWeek = DayOfWeek.Monday;
    
    // 2005/01/31 23:04:05
    dateTime = new DateTime(2005 , 01 , 31 , 23, 4 , 5 , calendar);

    infoText = DateTimeInfoHelper.GetInfo(
        calendar ,
        calendarWeekRule ,
        dayOfWeek ,
        dateTime
    );

    Console.WriteLine(infoText);
    counter++;

    Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
}
```

might ouput following.

```
In TestMethod1 method call,
///* --------- Example 1 --------- *///
Is the DateTime `06/13/2025 11:11:40 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:11
Current Minute:11
Current Second:40
Current Millisecond:940
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 2 --------- *///
Is the DateTime `01/31/2005 0:00:00 +08:00` read only?`True`
Current Era:1
Current Year:2005
Current Month:1
Current Week Of Year:5
Current Day Of Year:31
Current Day Of Month:31
Current Day Of Week:Monday
Current Days In Year:365
Current Days In Month:31
Current Hour:0
Current Minute:0
Current Second:0
Current Millisecond:0
Current CalendarAlgorithmType:SolarCalendar
Is year 2005 a leap year?`False`
Is month 1 of year 2005 a leap month?`False`
Is 31th day of month 1 of year 2005 a leap month?`False`

///* --------- Example 3 --------- *///
Is the DateTime `01/31/2005 23:04:05 +08:00` read only?`True`
Current Era:1
Current Year:2005
Current Month:1
Current Week Of Year:5
Current Day Of Year:31
Current Day Of Month:31
Current Day Of Week:Monday
Current Days In Year:365
Current Days In Month:31
Current Hour:23
Current Minute:4
Current Second:5
Current Millisecond:0
Current CalendarAlgorithmType:SolarCalendar
Is year 2005 a leap year?`False`
Is month 1 of year 2005 a leap month?`False`
Is 31th day of month 1 of year 2005 a leap month?`False`
```

## reference
### API docs
+ [`Calendar Class`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar?view=net-9.0)