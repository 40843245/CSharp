# CH6 -- get an array of all the index parameters of a property
## objectives
You will learn how to

+ get an array of all the index parameters of a property

## CH6.1 -- get an array of all the index parameters of a property
### `GetIndexParameters` instance method
returns an array of all the index parameters for the property.

## examples
### example 1
#### Beans

`Video.cs`

```
namespace Example.Beans
{
    public class Video
    {
        private string caption = "A Default caption";
        public string Caption
        {
            get { return caption; }
            set
            {
                if(caption != value) 
                { 
                    caption = value; 
                }
            }
        }

        public string Name { get; set; }

        private string [ ] strings = { "abc" , "def" , "ghi" , "jkl" };
        public string this [ int Index ]
        {
            get
            {
                return strings [ Index ];
            }
            set
            {
                strings [ Index ] = value;
            }
        }
    }
}
```
#### main code
Invoking following method

```
        /// <summary>
        /// get an array of all the index parameters of a property.
        /// </summary>
        public static void TestMethod8()
        {
            Console.WriteLine("In {0} nethod call," , MethodBase.GetCurrentMethod().Name);

            string propertyNameToFind = string.Empty;

            Type type;
            PropertyInfo propertyInfo;
            ParameterInfo [ ] parameterInfos;

            string text = string.Empty;
            Video mySenpaiIsAnnoyingThemeSong = new Video()
            {
                Name = "My Senpai Is Annoying",
            };

            Person personNico = new Person()
            {
                FirstName = "Yazawa" ,
                LastName = "Nico" ,
            };
            int counter = 1;

            ///* ------ Example 1 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "Caption";

            type = mySenpaiIsAnnoyingThemeSong.GetType();
            propertyInfo = type.GetProperty(propertyNameToFind);


            if(propertyInfo != null)
            {
                parameterInfos = propertyInfo.GetIndexParameters();
                text = $"The {propertyNameToFind} property is found\n";
                text += $"{type.FullName}.{propertyInfo.Name} has {parameterInfos.GetLength(0)}  parameters.\n";
                foreach(ParameterInfo parameterInfo in parameterInfos)
                {
                    text += $"   Parameter: {parameterInfo.Name}\n";
                }
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);


            counter++;

            ///* ------ Example 2 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "Caption";

            type = typeof(Video);
            propertyInfo = type.GetProperty(propertyNameToFind);


            if(propertyInfo != null)
            {
                parameterInfos = propertyInfo.GetIndexParameters();
                text = $"The {propertyNameToFind} property is found\n";
                text += $"{type.FullName}.{propertyInfo.Name} has {parameterInfos.GetLength(0)}  parameters.\n";
                foreach(ParameterInfo parameterInfo in parameterInfos)
                {
                    text += $"   Parameter: {parameterInfo.Name}\n";
                }
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);

            counter++;

            ///* ------ Example 3 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "Caption";

            type = Type.GetType("Example.Beans.Video");
            propertyInfo = type.GetProperty(propertyNameToFind);


            if(propertyInfo != null)
            {
                parameterInfos = propertyInfo.GetIndexParameters();
                text = $"The {propertyNameToFind} property is found\n";
                text += $"{type.FullName}.{propertyInfo.Name} has {parameterInfos.GetLength(0)}  parameters.\n";
                foreach(ParameterInfo parameterInfo in parameterInfos)
                {
                    text += $"   Parameter: {parameterInfo.Name}\n";
                }
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);

            counter++;

            ///* ------ Example 4 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "Item";

            type = mySenpaiIsAnnoyingThemeSong.GetType();
            propertyInfo = type.GetProperty(propertyNameToFind);


            if(propertyInfo != null)
            {
                parameterInfos = propertyInfo.GetIndexParameters();
                text = $"The {propertyNameToFind} property is found\n";
                text += $"{type.FullName}.{propertyInfo.Name} has {parameterInfos.GetLength(0)}  parameters.\n";
                foreach(ParameterInfo parameterInfo in parameterInfos)
                {
                    text += $"   Parameter: {parameterInfo.Name}\n";
                }
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);

            counter++;

            ///* ------ Example 5 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "Item";

            type = typeof(Video);
            propertyInfo = type.GetProperty(propertyNameToFind);


            if(propertyInfo != null)
            {
                parameterInfos = propertyInfo.GetIndexParameters();
                text = $"The {propertyNameToFind} property is found\n";
                text += $"{type.FullName}.{propertyInfo.Name} has {parameterInfos.GetLength(0)}  parameters.\n";
                foreach(ParameterInfo parameterInfo in parameterInfos)
                {
                    text += $"   Parameter: {parameterInfo.Name}\n";
                }
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);

            counter++;

            ///* ------ Example 6 ------ *///
            Console.WriteLine(@"///* ------ Example {0} ------ *///" , counter);

            propertyNameToFind = "Item";

            type = Type.GetType("Example.Beans.Video");
            propertyInfo = type.GetProperty(propertyNameToFind);


            if(propertyInfo != null)
            {
                parameterInfos = propertyInfo.GetIndexParameters();
                text = $"The {propertyNameToFind} property is found\n";
                text += $"{type.FullName}.{propertyInfo.Name} has {parameterInfos.GetLength(0)}  parameters.\n";
                foreach(ParameterInfo parameterInfo in parameterInfos)
                {
                    text += $"   Parameter: {parameterInfo.Name}\n";
                }
            }
            else
            {
                text = $"The {propertyNameToFind} property is NOT found\n";
            }

            Console.WriteLine(text);

            counter++;
        }
```

will output following

```
In TestMethod8 nethod call,
///* ------ Example 1 ------ *///
The Caption property is found
Example.Beans.Video.Caption has 0  parameters.

///* ------ Example 2 ------ *///
The Caption property is found
Example.Beans.Video.Caption has 0  parameters.

///* ------ Example 3 ------ *///
The Caption property is found
Example.Beans.Video.Caption has 0  parameters.

///* ------ Example 4 ------ *///
The Item property is found
Example.Beans.Video.Item has 1  parameters.
   Parameter: Index

///* ------ Example 5 ------ *///
The Item property is found
Example.Beans.Video.Item has 1  parameters.
   Parameter: Index

///* ------ Example 6 ------ *///
The Item property is found
Example.Beans.Video.Item has 1  parameters.
   Parameter: Index

```

## reference
### API docs
+ [`PropertyInfo.GetIndexParameters Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getindexparameters?view=net-9.0)

### examples
+ The code of example 1 is referenced from [`PropertyInfo.GetIndexParameters Method`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.propertyinfo.getindexparameters?view=net-9.0) and is modified.