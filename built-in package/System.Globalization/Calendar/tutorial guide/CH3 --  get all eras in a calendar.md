# CH3 -- get all eras in a calendar
## objectives
You will learn how to 

+ get all eras in a calendar

## CH3.1 -- get all eras in a calendar
### `Eras` getter-property

```
JapaneseCalendar japaneseCalendar = new JapaneseCalendar();

int [] japaneseCalendar.Eras;
```

returs the array of era (whose type is `int[]`) in respective order.

## examples
### exaple 1
#### main code

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + get all eras in Japense Calendar.
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // Creates and initializes a JapaneseCalendar.
            JapaneseCalendar japaneseCalendar = new JapaneseCalendar();

            // Displays the values in the Eras property.
            for(int i = 0; i < japaneseCalendar.Eras.Length; i++)
            {
                Console.WriteLine("Eras[{0}] = {1}" , i , japaneseCalendar.Eras [ i ]);
            }
        }
```

might output as following

```
In TestMethod2 method call,
Eras[0] = 5
Eras[1] = 4
Eras[2] = 3
Eras[3] = 2
Eras[4] = 1
```

## reference 
### API docs
+ [`Calendar.Eras Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.calendar.eras?view=net-9.0)
