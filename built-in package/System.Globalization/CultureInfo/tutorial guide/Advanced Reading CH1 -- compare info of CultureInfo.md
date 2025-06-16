# Advanced Reading CH1 -- compare info of CultureInfo
## objectives
You will learn how to 

+ compare info of CultureInfo

## Advanced Reading CH1.1 -- compare info of CultureInfo
### `CompareInfo` getter property
gets the [CompareInfo](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.compareinfo?view=net-8.0) that defines how to compare strings for the culture.

## examples
### example 1
#### extension methods

`GetInfoWithInternationalCultureAndTraditionCulture` extension method is defined in `CultureInfoExtensionMethods` static class

definition of `GetInfoWithInternationalCultureAndTraditionCulture` 

```
        public static string GetInfoWithInternationalCultureAndTraditionCulture(
            this CultureInfo internationalCulture,
            CultureInfo traditionalCulture
        )
        {
            StringBuilder stringBuilder = new StringBuilder();

            // Displays the properties of each culture.
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "PROPERTY" , "INTERNATIONAL" , "TRADITIONAL");
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "CompareInfo" , internationalCulture.CompareInfo , traditionalCulture.CompareInfo);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "DisplayName" , internationalCulture.DisplayName , traditionalCulture.DisplayName);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "EnglishName" , internationalCulture.EnglishName , traditionalCulture.EnglishName);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "IsNeutralCulture" , internationalCulture.IsNeutralCulture , traditionalCulture.IsNeutralCulture);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "IsReadOnly" , internationalCulture.IsReadOnly , traditionalCulture.IsReadOnly);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "LCID" , internationalCulture.LCID , traditionalCulture.LCID);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "Name" , internationalCulture.Name , traditionalCulture.Name);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "NativeName" , internationalCulture.NativeName , traditionalCulture.NativeName);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "Parent" , internationalCulture.Parent , traditionalCulture.Parent);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "TextInfo" , internationalCulture.TextInfo , traditionalCulture.TextInfo);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "ThreeLetterISOLanguageName" , internationalCulture.ThreeLetterISOLanguageName , traditionalCulture.ThreeLetterISOLanguageName);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "ThreeLetterWindowsLanguageName" , internationalCulture.ThreeLetterWindowsLanguageName , traditionalCulture.ThreeLetterWindowsLanguageName);
            stringBuilder.AppendFormat("{0,-31}{1,-47}{2,-25}\n" , "TwoLetterISOLanguageName" , internationalCulture.TwoLetterISOLanguageName , traditionalCulture.TwoLetterISOLanguageName);
            stringBuilder.AppendLine();

            // Compare two strings using internationalCulture.
            stringBuilder.AppendFormat("Comparing \"llegar\" and \"lugar\"\n");
            stringBuilder.AppendFormat("   With internationalCulture.CompareInfo.Compare: {0}\n" , internationalCulture.CompareInfo.Compare("llegar" , "lugar"));
            stringBuilder.AppendFormat("   With traditionalCulture.CompareInfo.Compare: {0}\n" , traditionalCulture.CompareInfo.Compare("llegar" , "lugar"));

            return stringBuilder.ToString();
        }
```
#### main code
Invoke following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + access propeties of `CultereInfo`
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            string infoText;

            // Creates and initializes the CultureInfo which uses the international sort.
            CultureInfo myInternationCulture = new CultureInfo("es-ES" , false);

            // Creates and initializes the CultureInfo which uses the traditional sort.
            CultureInfo myTraditionalCulture = new CultureInfo(0x040A , false);

            infoText = myInternationCulture.GetInfoWithInternationalCultureAndTraditionCulture(myTraditionalCulture);
            
            Console.WriteLine(infoText);
        }
```

will output following

```
In TestMethod1 method call
PROPERTY                       INTERNATIONAL                                  TRADITIONAL
CompareInfo                    CompareInfo - es-ES                            CompareInfo - es-ES_tradnl
DisplayName                    Spanish (Spain)                                Spanish (Spain)
EnglishName                    Spanish (Spain)                                Spanish (Spain, Sort Order=tradnl)
IsNeutralCulture               False                                          False
IsReadOnly                     False                                          False
LCID                           3082                                           1034
Name                           es-ES                                          es-ES
NativeName                     espanol (Espana)                               Spanish (Spain, Sort Order=tradnl)
Parent                         es                                             es-ES
TextInfo                       TextInfo - es-ES                               TextInfo - es-ES_tradnl
ThreeLetterISOLanguageName     spa                                            spa
ThreeLetterWindowsLanguageName ESN                                            ESP
TwoLetterISOLanguageName       es                                             es

Comparing "llegar" and "lugar"
   With internationalCulture.CompareInfo.Compare: -1
   With traditionalCulture.CompareInfo.Compare: -1

```

## reference
### API docs
+ [`CultureInfo.CompareInfo Property`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.compareinfo?view=net-8.0)

### examples
The code in example 1 is referenced from the first example in [`CultureInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=net-8.0)
