# CH8 -- MaxSupportedDateTime and MinSupportedDateTime
## objectives
You will know what is

+ `MaxSupportedDateTime`

+ `MinSupportedDateTime`

in derived class of `Calender` instance.

### `MaxSupportedDateTime` getter property
return the latest date and time supported by this calendar. 

The default is `DateTime.MaxValue` which 

is defined in abstract class `Calender`.

[source code of `Calender.cs` which defines `MaxSupportedDateTime`](https://github.com/dotnet/runtime/blob/5535e31a712343a63f5d7d796cd874e563e5ac14/src/libraries/System.Private.CoreLib/src/System/Globalization/Calendar.cs#L64C57-L64C74)

### `MinSupportedDateTime` getter property
return the earliest date and time supported by this calendar. 

The default is `DateTime.MinValue` which 

is defined in abstract class `Calender`.

[source code of `Calender.cs` which defines `MinSupportedDateTime`](https://github.com/dotnet/runtime/blob/5535e31a712343a63f5d7d796cd874e563e5ac14/src/libraries/System.Private.CoreLib/src/System/Globalization/Calendar.cs#L62C57-L62C74)

## examples
### example 1
Invoking following method

```
       /// <summary>
       /// illustrate 
       /// 
       /// + `MaxSupportedDateTime` getter property
       /// 
       /// + `MinSupportedDateTime` getter property
       /// </summary>
       public static void TestMethod5()
       {
           Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

           string dateTimeFormat = 
               DateTimeConstants.DateTimeFormatConstants.CustomDateTimeFormatConstants. ANNO_DOMINI_WITH_TIMEZONE_OFFSET_FORMAT;

           CultureInfo currentCultureInfo = CultureInfo.CurrentCulture;
           Calendar currentCultureInfoCalendar = currentCultureInfo.Calendar;

           ChineseLunisolarCalendar chineseLunisolarCalendar = new ChineseLunisolarCalendar();
           JapaneseLunisolarCalendar japaneseLunisolarCalendar = new JapaneseLunisolarCalendar();
           KoreanLunisolarCalendar koreanLunisolarCalendar = new KoreanLunisolarCalendar();
           KoreanCalendar koreanCalendar = new KoreanCalendar();
           JapaneseCalendar japaneseCalendar = new JapaneseCalendar();
           GregorianCalendar gregorianCalendar = new GregorianCalendar();
           HijriCalendar hijriCalendar = new HijriCalendar();
           UmAlQuraCalendar umAlQuraCalendar = new UmAlQuraCalendar();

           Calendar [ ] calendarArray = new Calendar [ ]
           {
               chineseLunisolarCalendar, // not read only
               japaneseLunisolarCalendar, // not read only
               koreanLunisolarCalendar, // not read only
               koreanCalendar, // not read only
               japaneseCalendar, // not read only
               gregorianCalendar, // not read only
               hijriCalendar, // not read only
               umAlQuraCalendar, // not read only
               currentCultureInfoCalendar, // read only
           };

           for(int i = 0; i < calendarArray.Length; i++)
           {
               ///* --------- Example {i+1} --------- *///
               Console.WriteLine("///* --------- Example {0} --------- *///" , i + 1);

               Calendar calendar = calendarArray [ i ];
               
               Console.WriteLine("About the calendar `{0}` info,",calendar.ToString());
               Console.WriteLine("\tMaxSupportedDateTime getter property of calendar:`{0}`," , calendar.MaxSupportedDateTime.ToString(dateTimeFormat));
               Console.WriteLine("\tMinSupportedDateTime getter property of calendar:`{0}`," , calendar.MinSupportedDateTime.ToString(dateTimeFormat));
               Console.WriteLine("\tDateTime.MaxValue static field:`{0}`," , DateTime.MaxValue.ToString(dateTimeFormat));
               Console.WriteLine("\tDateTime.MinValue static field:`{0}`," , DateTime.MinValue.ToString(dateTimeFormat));

               Console.WriteLine("Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`{0}`" , calendar.MaxSupportedDateTime.Equals(DateTime.MaxValue));
               Console.WriteLine("Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`{0}`" , calendar.MaxSupportedDateTime == DateTime.MaxValue);

               Console.WriteLine("Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`{0}`" , calendar.MinSupportedDateTime.Equals(DateTime.MinValue));
               Console.WriteLine("Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`{0}`" , calendar.MinSupportedDateTime == DateTime.MinValue);
           }
       }
```

will output following

```
In TestMethod5 method call,
///* --------- Example 1 --------- *///
About the calendar `System.Globalization.ChineseLunisolarCalendar` info,
        MaxSupportedDateTime getter property of calendar:`01/28/2101 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`02/19/1901 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
///* --------- Example 2 --------- *///
About the calendar `System.Globalization.JapaneseLunisolarCalendar` info,
        MaxSupportedDateTime getter property of calendar:`01/22/2050 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`01/28/1960 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
///* --------- Example 3 --------- *///
About the calendar `System.Globalization.KoreanLunisolarCalendar` info,
        MaxSupportedDateTime getter property of calendar:`02/10/2051 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`02/19/0918 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
///* --------- Example 4 --------- *///
About the calendar `System.Globalization.KoreanCalendar` info,
        MaxSupportedDateTime getter property of calendar:`12/31/9999 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`01/01/0001 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
///* --------- Example 5 --------- *///
About the calendar `System.Globalization.JapaneseCalendar` info,
        MaxSupportedDateTime getter property of calendar:`12/31/9999 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`09/08/1868 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
///* --------- Example 6 --------- *///
About the calendar `System.Globalization.GregorianCalendar` info,
        MaxSupportedDateTime getter property of calendar:`12/31/9999 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`01/01/0001 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
///* --------- Example 7 --------- *///
About the calendar `System.Globalization.HijriCalendar` info,
        MaxSupportedDateTime getter property of calendar:`12/31/9999 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`07/18/0622 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
///* --------- Example 8 --------- *///
About the calendar `System.Globalization.UmAlQuraCalendar` info,
        MaxSupportedDateTime getter property of calendar:`11/16/2077 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`04/30/1900 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`False`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`False`
///* --------- Example 9 --------- *///
About the calendar `System.Globalization.GregorianCalendar` info,
        MaxSupportedDateTime getter property of calendar:`12/31/9999 23:59:59 +08:00`,
        MinSupportedDateTime getter property of calendar:`01/01/0001 0:00:00 +08:00`,
        DateTime.MaxValue static field:`12/31/9999 23:59:59 +08:00`,
        DateTime.MinValue static field:`01/01/0001 0:00:00 +08:00`,
Is MaxSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MaxSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar referentially equals to DateTime.MaxValue static field?`True`
Is MinSupportedDateTime getter property of calendar non-referentially equals to DateTime.MaxValue static field?`True`
```

## reference
### API docs
+ [`Calendar.MaxSupportedDateTime Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.maxsupporteddatetime?view=net-8.0)

+ [`Calendar.MinSupportedDateTime Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.minsupporteddatetime?view=net-8.0)

### source code
#### `Calender.cs`

+ About `MaxSupportedDateTime`,

[source code of `Calender.cs` which defines `MaxSupportedDateTime`](https://github.com/dotnet/runtime/blob/5535e31a712343a63f5d7d796cd874e563e5ac14/src/libraries/System.Private.CoreLib/src/System/Globalization/Calendar.cs#L64C57-L64C74)

+ About `MinSupportedDateTime`, 

[source code of `Calender.cs` which defines `MinSupportedDateTime`](https://github.com/dotnet/runtime/blob/5535e31a712343a63f5d7d796cd874e563e5ac14/src/libraries/System.Private.CoreLib/src/System/Globalization/Calendar.cs#L62C57-L62C74)