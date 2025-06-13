# CH9 -- convert two-digit year to four-digit year
## objectives
You will learn how to

+ convert two-digit year to four-digit year

## CH9 -- convert two-digit year to four-digit year
### `ToFourDigitYear` instance method

```
int twoDigitsYear = 01;
int fourDigitsYear = currentCultureInfoCalendar.ToFourDigitYear(twoDigitsYear);
```

it will converts two-digit `01` to four-digits.

### Rule
Given a two-digit year n,

+ Case 1: 

If n >= 0 and n < 50, 

then it returns (2000 + n).

+ Case 2:

If n >= 50 and n <= 100,

then it returns (1900 + n).

+ Case 3:

If n >= 100 and n <= `Int32.MaxValue`,

then it will return n.

+ Case 4:

If n < 0,

then it will throw an exception at runtime.

see following example 1 as demo.

I have tested these use cases 

from n = -100 to n = 99999,

you can prove it by modified the following example 1.

## examples
### example 1
#### main code
Invoking following method

```
       /// <summary>
       /// illustrate how to
       /// 
       /// + convert two-digit year to four-digit year
       /// </summary>
       public static void TestMethod6()
       {
           Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

           CultureInfo currentCultureInfo = CultureInfo.CurrentCulture;
           Calendar currentCultureInfoCalendar = currentCultureInfo.Calendar;

           for(int twoDigitsYear = 01; twoDigitsYear < 100; twoDigitsYear ++)
           {
               int fourDigitsYear = currentCultureInfoCalendar.ToFourDigitYear(twoDigitsYear);
               Console.WriteLine("The year with last two digit {0:D2} represents year {1} in A.D.",twoDigitsYear,fourDigitsYear);
           }
       }
```

will output following

```
In TestMethod6 method call,
The year with last two digit 01 represents year 2001 in A.D.
The year with last two digit 02 represents year 2002 in A.D.
The year with last two digit 03 represents year 2003 in A.D.
The year with last two digit 04 represents year 2004 in A.D.
The year with last two digit 05 represents year 2005 in A.D.
The year with last two digit 06 represents year 2006 in A.D.
The year with last two digit 07 represents year 2007 in A.D.
The year with last two digit 08 represents year 2008 in A.D.
The year with last two digit 09 represents year 2009 in A.D.
The year with last two digit 10 represents year 2010 in A.D.
The year with last two digit 11 represents year 2011 in A.D.
The year with last two digit 12 represents year 2012 in A.D.
The year with last two digit 13 represents year 2013 in A.D.
The year with last two digit 14 represents year 2014 in A.D.
The year with last two digit 15 represents year 2015 in A.D.
The year with last two digit 16 represents year 2016 in A.D.
The year with last two digit 17 represents year 2017 in A.D.
The year with last two digit 18 represents year 2018 in A.D.
The year with last two digit 19 represents year 2019 in A.D.
The year with last two digit 20 represents year 2020 in A.D.
The year with last two digit 21 represents year 2021 in A.D.
The year with last two digit 22 represents year 2022 in A.D.
The year with last two digit 23 represents year 2023 in A.D.
The year with last two digit 24 represents year 2024 in A.D.
The year with last two digit 25 represents year 2025 in A.D.
The year with last two digit 26 represents year 2026 in A.D.
The year with last two digit 27 represents year 2027 in A.D.
The year with last two digit 28 represents year 2028 in A.D.
The year with last two digit 29 represents year 2029 in A.D.
The year with last two digit 30 represents year 2030 in A.D.
The year with last two digit 31 represents year 2031 in A.D.
The year with last two digit 32 represents year 2032 in A.D.
The year with last two digit 33 represents year 2033 in A.D.
The year with last two digit 34 represents year 2034 in A.D.
The year with last two digit 35 represents year 2035 in A.D.
The year with last two digit 36 represents year 2036 in A.D.
The year with last two digit 37 represents year 2037 in A.D.
The year with last two digit 38 represents year 2038 in A.D.
The year with last two digit 39 represents year 2039 in A.D.
The year with last two digit 40 represents year 2040 in A.D.
The year with last two digit 41 represents year 2041 in A.D.
The year with last two digit 42 represents year 2042 in A.D.
The year with last two digit 43 represents year 2043 in A.D.
The year with last two digit 44 represents year 2044 in A.D.
The year with last two digit 45 represents year 2045 in A.D.
The year with last two digit 46 represents year 2046 in A.D.
The year with last two digit 47 represents year 2047 in A.D.
The year with last two digit 48 represents year 2048 in A.D.
The year with last two digit 49 represents year 2049 in A.D.
The year with last two digit 50 represents year 1950 in A.D.
The year with last two digit 51 represents year 1951 in A.D.
The year with last two digit 52 represents year 1952 in A.D.
The year with last two digit 53 represents year 1953 in A.D.
The year with last two digit 54 represents year 1954 in A.D.
The year with last two digit 55 represents year 1955 in A.D.
The year with last two digit 56 represents year 1956 in A.D.
The year with last two digit 57 represents year 1957 in A.D.
The year with last two digit 58 represents year 1958 in A.D.
The year with last two digit 59 represents year 1959 in A.D.
The year with last two digit 60 represents year 1960 in A.D.
The year with last two digit 61 represents year 1961 in A.D.
The year with last two digit 62 represents year 1962 in A.D.
The year with last two digit 63 represents year 1963 in A.D.
The year with last two digit 64 represents year 1964 in A.D.
The year with last two digit 65 represents year 1965 in A.D.
The year with last two digit 66 represents year 1966 in A.D.
The year with last two digit 67 represents year 1967 in A.D.
The year with last two digit 68 represents year 1968 in A.D.
The year with last two digit 69 represents year 1969 in A.D.
The year with last two digit 70 represents year 1970 in A.D.
The year with last two digit 71 represents year 1971 in A.D.
The year with last two digit 72 represents year 1972 in A.D.
The year with last two digit 73 represents year 1973 in A.D.
The year with last two digit 74 represents year 1974 in A.D.
The year with last two digit 75 represents year 1975 in A.D.
The year with last two digit 76 represents year 1976 in A.D.
The year with last two digit 77 represents year 1977 in A.D.
The year with last two digit 78 represents year 1978 in A.D.
The year with last two digit 79 represents year 1979 in A.D.
The year with last two digit 80 represents year 1980 in A.D.
The year with last two digit 81 represents year 1981 in A.D.
The year with last two digit 82 represents year 1982 in A.D.
The year with last two digit 83 represents year 1983 in A.D.
The year with last two digit 84 represents year 1984 in A.D.
The year with last two digit 85 represents year 1985 in A.D.
The year with last two digit 86 represents year 1986 in A.D.
The year with last two digit 87 represents year 1987 in A.D.
The year with last two digit 88 represents year 1988 in A.D.
The year with last two digit 89 represents year 1989 in A.D.
The year with last two digit 90 represents year 1990 in A.D.
The year with last two digit 91 represents year 1991 in A.D.
The year with last two digit 92 represents year 1992 in A.D.
The year with last two digit 93 represents year 1993 in A.D.
The year with last two digit 94 represents year 1994 in A.D.
The year with last two digit 95 represents year 1995 in A.D.
The year with last two digit 96 represents year 1996 in A.D.
The year with last two digit 97 represents year 1997 in A.D.
The year with last two digit 98 represents year 1998 in A.D.
The year with last two digit 99 represents year 1999 in A.D.
```

## reference
### API docs
+ [`Calendar.ToFourDigitYear(Int32) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.tofourdigityear?view=net-8.0)