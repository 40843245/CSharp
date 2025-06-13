# CH7 -- plus or minus time of `DateTime` using calendar
## objectives
You will learn how to

+ plus or minus time of `DateTime` using calendar

## CH7.1 -- plus or minus time of `DateTime` using calendar
### `AddYears` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddYears(dateTime,1);
```

will plus 1 year to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddYears(dateTime,-1);
```

will minus 1 year to `dateTime` then return it.

### `AddMonths` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddMonths(dateTime,1);
```

will plus 1 month to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddMonths(dateTime,-1);
```

will minus 1 month to `dateTime` then return it.

### `AddWeeks` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddWeeks(dateTime,1);
```

will plus 1 week to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddWeeks(dateTime,-1);
```

will minus 1 week to `dateTime` then return it.

### `AddDays` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddDays(dateTime,1);
```

will plus 1 day to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddDays(dateTime,-1);
```

will minus 1 day to `dateTime` then return it.

### `AddHours` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddHours(dateTime,1);
```

will plus 1 hour to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddHours(dateTime,-1);
```

will minus 1 hour to `dateTime` then return it.

### `AddMinutes` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddMinutes(dateTime,1);
```

will plus 1 minute to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddMinutes(dateTime,-1);
```

will minus 1 minute to `dateTime` then return it.

### `AddSeconds` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddSeconds(dateTime,1);
```

will plus 1 second to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddSeconds(dateTime,-1);
```

will minus 1 second to `dateTime` then return it.

### `AddMilliseconds` instance method

```
Date newDateTime =
currentCultureInfoCalendar.AddMilliseconds(dateTime,1);
```

will plus 1 millisecond (`1/1000` second) to `dateTime` then return it.

```
Date newDateTime =
currentCultureInfoCalendar.AddMilliseconds(dateTime,-1);
```

will minus 1 millisecond (`1/1000` second) to `dateTime` then return it.

## examples
### example 1
#### helper class
`DateTimeInfoHelper` helper class is defined in `DateTimeInfoHelper.cs` which appears at example 1 in CH2.

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + plus or minus time to specified `DateTime` instance 
        /// </summary>
        public static void TestMethod4()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            CultureInfo currentCultureInfo = CultureInfo.CurrentCulture;
            Calendar currentCultureInfoCalendar = currentCultureInfo.Calendar;

            string infoText = string.Empty;

            DateTime dateTime;

            CalendarWeekRule calendarWeekRule;
            DayOfWeek dayOfWeek;

            calendarWeekRule = CalendarWeekRule.FirstFullWeek;
            dayOfWeek = DayOfWeek.Monday;
            dateTime = DateTime.Now; // current `DateTime`

            DateTime [ ] datas = new DateTime [ ]
            {
                dateTime, // current `DateTime`
                currentCultureInfoCalendar.AddYears(dateTime,1), // current `DateTime` plus 1 year
                currentCultureInfoCalendar.AddMonths(dateTime,1), // current `DateTime` plus 1 month
                currentCultureInfoCalendar.AddWeeks(dateTime,1), // current `DateTime` plus 1 week
                currentCultureInfoCalendar.AddDays(dateTime,1), // current `DateTime` plus 1 day
                currentCultureInfoCalendar.AddHours(dateTime,1), // current `DateTime` plus 1 hour
                currentCultureInfoCalendar.AddMinutes(dateTime,1), // current `DateTime` plus 1 minute
                currentCultureInfoCalendar.AddSeconds(dateTime,1), // current `DateTime` plus 1 second
                currentCultureInfoCalendar.AddMilliseconds(dateTime,1), // current `DateTime` plus 1 millisecond
                currentCultureInfoCalendar.AddYears(dateTime,-1), // current `DateTime` minus 1 year
                currentCultureInfoCalendar.AddMonths(dateTime,-1), // current `DateTime` minus 1 month
                currentCultureInfoCalendar.AddWeeks(dateTime,-1), // current `DateTime` minus 1 week
                currentCultureInfoCalendar.AddDays(dateTime,-1), // current `DateTime` minus 1 day
                currentCultureInfoCalendar.AddHours(dateTime,-1), // current `DateTime` minus 1 hour
                currentCultureInfoCalendar.AddMinutes(dateTime,-1), // current `DateTime` minus 1 minute
                currentCultureInfoCalendar.AddSeconds(dateTime,-1), // current `DateTime` minus 1 second
                currentCultureInfoCalendar.AddMilliseconds(dateTime,-1), // current `DateTime` minus 1 millisecond
            };

            for(int i = 0; i < datas.Length; i++)
            {
                ///* --------- Example {i+1} --------- *///
                Console.WriteLine("///* --------- Example {0} --------- *///" , i + 1);

                DateTime data = datas[i];
                infoText = DateTimeInfoHelper.GetInfo(
                    currentCultureInfoCalendar ,
                    calendarWeekRule ,
                    dayOfWeek ,
                    data
                );

                Console.WriteLine(infoText);
            }
        }
```

will output following

```
In TestMethod4 method call,
///* --------- Example 1 --------- *///
Is the DateTime `06/13/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 2 --------- *///
Is the DateTime `06/13/2026 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2026
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Saturday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2026 a leap year?`False`
Is month 6 of year 2026 a leap month?`False`
Is 30th day of month 6 of year 2026 a leap month?`False`

///* --------- Example 3 --------- *///
Is the DateTime `07/13/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:7
Current Week Of Year:27
Current Day Of Year:194
Current Day Of Month:13
Current Day Of Week:Sunday
Current Days In Year:365
Current Days In Month:31
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 7 of year 2025 a leap month?`False`
Is 31th day of month 7 of year 2025 a leap month?`False`

///* --------- Example 4 --------- *///
Is the DateTime `06/20/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:24
Current Day Of Year:171
Current Day Of Month:20
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 5 --------- *///
Is the DateTime `06/14/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:165
Current Day Of Month:14
Current Day Of Week:Saturday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 6 --------- *///
Is the DateTime `06/13/2025 14:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:14
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 7 --------- *///
Is the DateTime `06/13/2025 13:05:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:5
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 8 --------- *///
Is the DateTime `06/13/2025 13:04:02 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:2
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 9 --------- *///
Is the DateTime `06/13/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:60
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 10 --------- *///
Is the DateTime `06/13/2024 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2024
Current Month:6
Current Week Of Year:24
Current Day Of Year:165
Current Day Of Month:13
Current Day Of Week:Thursday
Current Days In Year:366
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2024 a leap year?`True`
Is month 6 of year 2024 a leap month?`False`
Is 30th day of month 6 of year 2024 a leap month?`False`

///* --------- Example 11 --------- *///
Is the DateTime `05/13/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:5
Current Week Of Year:19
Current Day Of Year:133
Current Day Of Month:13
Current Day Of Week:Tuesday
Current Days In Year:365
Current Days In Month:31
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 5 of year 2025 a leap month?`False`
Is 31th day of month 5 of year 2025 a leap month?`False`

///* --------- Example 12 --------- *///
Is the DateTime `06/06/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:22
Current Day Of Year:157
Current Day Of Month:6
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 13 --------- *///
Is the DateTime `06/12/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:163
Current Day Of Month:12
Current Day Of Week:Thursday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 14 --------- *///
Is the DateTime `06/13/2025 12:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:12
Current Minute:4
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 15 --------- *///
Is the DateTime `06/13/2025 13:03:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:3
Current Second:1
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 16 --------- *///
Is the DateTime `06/13/2025 13:04:00 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:0
Current Millisecond:59
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

///* --------- Example 17 --------- *///
Is the DateTime `06/13/2025 13:04:01 +08:00` read only?`True`
Current Era:1
Current Year:2025
Current Month:6
Current Week Of Year:23
Current Day Of Year:164
Current Day Of Month:13
Current Day Of Week:Friday
Current Days In Year:365
Current Days In Month:30
Current Hour:13
Current Minute:4
Current Second:1
Current Millisecond:58
Current CalendarAlgorithmType:SolarCalendar
Is year 2025 a leap year?`False`
Is month 6 of year 2025 a leap month?`False`
Is 30th day of month 6 of year 2025 a leap month?`False`

```

## reference
### API docs
+ [`Calendar.AddYears(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addyears?view=net-9.0)

+ [`Calendar.AddMonths(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addmonths?view=net-9.0)

+ [`Calendar.AddWeeks(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addweeks?view=net-9.0)

+ [`Calendar.AddDays(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.adddays?view=net-9.0)

+ [`Calendar.AddHours(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addhours?view=net-9.0)

+ [`Calendar.AddMinutes(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addminutes?view=net-9.0)

+ [`Calendar.AddSeconds(DateTime, Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addseconds?view=net-9.0)

+ [`Calendar.AddMilliseconds(DateTime, Double) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.addmilliseconds?view=net-9.0)