# CH6 -- memberwise clone Calendar instance but set it as read only
## objectives
You will learn how to

+ memberwise clone Calendar instance but set it as read only

## CH6.1 -- memberwise clone Calendar instance but set it as read only
### `Calendar.ReadOnly` static method

```
JapaneseLunisolarCalendar japaneseLunisolarCalendar = new JapaneseLunisolarCalendar();
Calendar readonlyCalendar = Calendar.ReadOnly(japaneseLunisolarCalendar); // read only
```

it will memberwise clone `japaneseLunisolarCalendar` instance 

but set cloned calender as read only and then assign it to `readonlyCalendar` instance.

all getter property (except `IsReadOnly)` of `japaneseLunisolarCalendar` 

will return value which are same as `readonlyCalendar` instance.

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + Memberwise clone a `Calendar` instance but set it as read only.
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

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

            foreach(Calendar item in calendarArray)
            {
                Console.WriteLine("+ Is the `{0}` calendar read only?`{1}`" , item.ToString() , item.IsReadOnly);
                Calendar readonlyCalendar = Calendar.ReadOnly(item); // read only
                Console.WriteLine("After memberwise cloning a `Calendar` instance `{0}` but setting it as read only.",item.ToString());
                Console.WriteLine("+ Is the `{0}` calendar read only?`{1}`" , readonlyCalendar.ToString() , readonlyCalendar.IsReadOnly);
            }
        }
```

will output following

```
In TestMethod3 method call,
+ Is the `System.Globalization.ChineseLunisolarCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.ChineseLunisolarCalendar` but setting it as read only.
+ Is the `System.Globalization.ChineseLunisolarCalendar` calendar read only?`True`
+ Is the `System.Globalization.JapaneseLunisolarCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.JapaneseLunisolarCalendar` but setting it as read only.
+ Is the `System.Globalization.JapaneseLunisolarCalendar` calendar read only?`True`
+ Is the `System.Globalization.KoreanLunisolarCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.KoreanLunisolarCalendar` but setting it as read only.
+ Is the `System.Globalization.KoreanLunisolarCalendar` calendar read only?`True`
+ Is the `System.Globalization.KoreanCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.KoreanCalendar` but setting it as read only.
+ Is the `System.Globalization.KoreanCalendar` calendar read only?`True`
+ Is the `System.Globalization.JapaneseCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.JapaneseCalendar` but setting it as read only.
+ Is the `System.Globalization.JapaneseCalendar` calendar read only?`True`
+ Is the `System.Globalization.GregorianCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.GregorianCalendar` but setting it as read only.
+ Is the `System.Globalization.GregorianCalendar` calendar read only?`True`
+ Is the `System.Globalization.HijriCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.HijriCalendar` but setting it as read only.
+ Is the `System.Globalization.HijriCalendar` calendar read only?`True`
+ Is the `System.Globalization.UmAlQuraCalendar` calendar read only?`False`
After memberwise cloning a `Calendar` instance `System.Globalization.UmAlQuraCalendar` but setting it as read only.
+ Is the `System.Globalization.UmAlQuraCalendar` calendar read only?`True`
+ Is the `System.Globalization.GregorianCalendar` calendar read only?`True`
After memberwise cloning a `Calendar` instance `System.Globalization.GregorianCalendar` but setting it as read only.
+ Is the `System.Globalization.GregorianCalendar` calendar read only?`True`
```

## reference
### API docs
+ [`Calendar.ReadOnly(Calendar) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.readonly?view=net-9.0)

## further reading
+ [Google Gemini's response -- `Calendar` instance that is NOT readonly (i.e. `IsReadOnly` property return `false`](https://g.co/gemini/share/73600c208766)