# CH10 -- set the last year in century range that is used for translating two-digit year to four-digit year
## objectives
You will learn how to

+ set the last year in century range that is used for translating two-digit year to four-digit year

## CH10.1 -- set the last year in century range that is used for translating two-digit year to four-digit year
### `TwoDigitYearMax` getter-setter property
allows a 2-digit year to be properly translated to a 4-digit year. 

For example, if this property is set to 2029 in (for `GregorianCalendar` instance), 

the 100-year range is from 1930 to 2029. 

Therefore, a 2-digit value of 30 is interpreted as 1930, 

while a 2-digit value of 29 is interpreted as 2029.

> [!NOTE]
> [Gregorian Calendar](https://en.wikipedia.org/wiki/Gregorian_calendar) is translated to [公曆](https://zh.wikipedia.org/wiki/%E5%85%AC%E5%8E%86) which is the general standard.

#### Remarks
+ If a `Calender` instance is read only,

when you try to set `TwoDigitYearMax` property, 

then it will throw an exception at runtime.

+ If `TwoDigitYearMax` is set to less than 99 or greater than 9999,

then it will throw an exception at runtime.

## examples
### example 1
The following example uses `GregorianCalendar` as `Calender` to

illustrate how `TwoDigitYearMax` property affects 

the way to convert two-digit year to four-digit year.

(the return value of `ToFourDigit` instance method call)

Invoking the following method will 

generated a directory `{0}` (where `{0}` is the current executing method name) (if it does NOT exist) under `~/AppData/Console/Output`, and repeat following steps

    Step 1. create `output ex{0:D6}.txt` (if it does NOT exist) under `~/AppData/Console/Output/{0}` 
    
    where 
    
    `{0}` in `~/AppData/Console/Output/{0}`, again, is the current executing method name

    and

    `{0:D6}` in `output ex{0:D6}.txt` is the current counter padding 0 at first with 6 digit 
    
    (such as `output ex000123.txt`).

    Step 2. 

    write text into that file.

method:

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + set the last year in century range that is used for 
        /// 
        /// translating two-digit year to four-digit year 
        /// 
        /// through setting `TwoDigitYearMax` getter-setter property.
        /// 
        /// Here, I will use `GregorianCalendar` instance as calendar.
        /// </summary>
        public static void TestMethod7()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string outputDirectory = PathConstants.DirectoryConstants.CONSOLE_OUTPUT;
            string testMethodName = MethodBase.GetCurrentMethod().Name;
            string testMethodDirectory = Path.Combine(outputDirectory , testMethodName);
            string outputFileFormattingString = "output ex{0:D6}.txt";
            string outputFileName;
            string outputFilePath;

            Directory.CreateDirectory(testMethodDirectory);

            GregorianCalendar gregorianCalendar = new GregorianCalendar();

            int counter = 1;
            for(
                int lastYearOfCenturyRange = 99;
                lastYearOfCenturyRange <= 9999;
                lastYearOfCenturyRange++, counter++
            )
            {
                ///* --------- Example {counter} --------- *///
                //Console.WriteLine("///* --------- Example {0} --------- *///" , counter);

                outputFileName = string.Format(outputFileFormattingString , counter);
                outputFilePath = Path.Combine(testMethodDirectory , outputFileName);

                FileHandler.CreateFile(outputFilePath);

                using(StreamWriter streamWriter = new StreamWriter(outputFilePath , false))
                {
                    streamWriter.WriteLine("///* --------- Example {0} --------- *///" , counter);
                    streamWriter.WriteLine("///* ----------- lastYearOfCenturyRange:{0}  ----------- *///" , lastYearOfCenturyRange);

                    gregorianCalendar.TwoDigitYearMax = lastYearOfCenturyRange;
                    streamWriter.WriteLine("After setting last year of a 100-year range to {0}," , gregorianCalendar.TwoDigitYearMax);

                    for(int twoDigitsYear = 1; twoDigitsYear < 100; twoDigitsYear++)
                    {
                        int fourDigitsYear = gregorianCalendar.ToFourDigitYear(twoDigitsYear);
                        streamWriter.WriteLine("+ The year with last two digit {0:D2} represents year {1} in A.D." , twoDigitsYear , fourDigitsYear);
                    }
                }
            }
        }
```

Output:

see `~/AppData/Console/Output/{0}` in demo project
    
where 
    
`{0}` in `~/AppData/Console/Output/{0}`, again, is the current executing method name.

