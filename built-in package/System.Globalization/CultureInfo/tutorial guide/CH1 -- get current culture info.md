# CH1 -- get current culture info
## objectives
You will learn how to

+ get current culture info

## CH1 -- get current culture info
### `CultureInfo.CurrentCulture` static getter -setter property

```
CultureInfo currentCultureInfo = CultureInfo.CurrentCulture;
```

will get current culture that used by the current thread and task-based asynchronous operations.

```
CultureInfo.CurrentCulture = new CultureInfo("es-ES", false);
```

will set current culture that used by the current thread and task-based asynchronous operations to Spain language with country code name (`es`).

## examples
### example 1
#### extension methods

`GetInfoWithInternationalCulture` extension method is defined in `CultureInfoExtensionMethods` static class.

definition of `GetInfoWithInternationalCulture` 

```
public static string GetInfoWithInternationalCulture(
    this CultureInfo cultureInfo
)
{
    StringBuilder stringBuilder = new StringBuilder();
    // Displays the properties of each culture.
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "PROPERTY" , "INTERNATIONAL");
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "CompareInfo" , cultureInfo.CompareInfo , cultureInfo.CompareInfo);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "DisplayName" , cultureInfo.DisplayName , cultureInfo.DisplayName);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "EnglishName" , cultureInfo.EnglishName , cultureInfo.EnglishName);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "IsNeutralCulture" , cultureInfo.IsNeutralCulture);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "IsReadOnly" ,cultureInfo.IsReadOnly);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "LCID" , cultureInfo.LCID);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "Name" ,  cultureInfo.Name);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "NativeName" , cultureInfo.NativeName);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "Parent" , cultureInfo.Parent);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "TextInfo" , cultureInfo.TextInfo);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "ThreeLetterISOLanguageName" , cultureInfo.ThreeLetterISOLanguageName);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "ThreeLetterWindowsLanguageName" , cultureInfo.ThreeLetterWindowsLanguageName);
    stringBuilder.AppendFormat("{0,-31}{1,-47}\n" , "TwoLetterISOLanguageName" , cultureInfo.TwoLetterISOLanguageName);
    stringBuilder.AppendLine();

    // Compare two strings using internationalCulture.
    stringBuilder.AppendFormat("Comparing \"llegar\" and \"lugar\"\n");
    stringBuilder.AppendFormat("   With cultureInfo.CompareInfo.Compare: {0}\n" , cultureInfo.CompareInfo.Compare("llegar" , "lugar"));

    return stringBuilder.ToString();
}
```

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to
        /// 
        /// + get current culture info
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            string infoText;

            CultureInfo currentCultureInfo = CultureInfo.CurrentCulture;

            infoText = currentCultureInfo.GetInfoWithInternationalCulture();

            Console.WriteLine(infoText);
        }
```

will output following

```
In TestMethod2 method call
PROPERTY                       INTERNATIONAL
CompareInfo                    CompareInfo - en-US
DisplayName                    English (United States)
EnglishName                    English (United States)
IsNeutralCulture               False
IsReadOnly                     True
LCID                           1033
Name                           en-US
NativeName                     English (United States)
Parent                         en
TextInfo                       TextInfo - en-US
ThreeLetterISOLanguageName     eng
ThreeLetterWindowsLanguageName ENU
TwoLetterISOLanguageName       en

Comparing "llegar" and "lugar"
   With cultureInfo.CompareInfo.Compare: -1

```

## reference
### API docs
+ [`CultureInfo.CurrentCulture`](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.currentculture?view=net-8.0)
