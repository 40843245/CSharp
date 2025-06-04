# CH4 -- default value of description of  `DescriptionAttribute`
## objectives
You will know

+ what is `DescriptionAttribute.Default`

## CH4.1 -- what is `DescriptionAttribute.Default`
`DescriptionAttribute.Default` is a `DescriptionAttribute` type whose description is a default value of string (empty string).

## examples
### example 1
#### main code
Invoking following method

```
        /// <summary>
        /// Default -- `DescriptionAttribute.Default`
        /// </summary>
        public static void TestMethod2()
        {
            Console.WriteLine("In {0} method call",MethodBase.GetCurrentMethod().Name);
            Console.WriteLine("Description of DescriptionAttribute.Default=`{0}`", DescriptionAttribute.Default.Description);
            Console.WriteLine("End of {0} method call",MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod2 method call
Description of DescriptionAttribute.Default=``
End of TestMethod2 method call
```

## reference
### API docs
+ [`DescriptionAttribute.Default Field`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.descriptionattribute.default?view=net-8.0#system-componentmodel-descriptionattribute-default)