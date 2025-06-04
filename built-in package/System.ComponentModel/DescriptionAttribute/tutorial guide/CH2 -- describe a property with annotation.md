# CH2 -- describe a property with annotation
## objectives
You will learn how to

+ describe a property with annotation

## CH2.1 -- describe a property with annotation
### `DescriptionAttribute` annotation 

In this example, 

it will describe two properties 

+ `PlayLists` with description `PlayLists that category all videos`
+ `Owner` with description `The info about owner of this channel in Social Media App`

```
using System.ComponentModel;

// ...
public class SociaMediaChannel
{
    [Description("PlayLists that category all videos")]
    public PlayLists PlayLists { get; init; }

    [Description("The info about owner of this channel in Social Media App")]
    public User Owner { get; init; }
}
```

> [!NOTE]
> `DescriptionAttribute` can be shortened as `Description`.

## examples
### example 1
#### extensions method
see demo project.

#### main code
Invoking following method

```
        /// <summary>
        /// illustrate 
        /// 
        /// + how to use `DescriptionAttribute` attribute inside annotation to describe the property.
        /// 
        /// + how to get the description of property. 
        /// </summary>
        public static void TestMethod1()
        {
            Console.WriteLine("In {0} method call",MethodBase.GetCurrentMethod().Name);

            DescriptionAttribute descriptionAttribute = null;
            string infoText = string.Empty;
            
            User userNico = new User()
            {
                Id = 1,
                Name = "Yazawa Nico",
            };

            PlayList playListNico = new PlayList()
            {
                Id = 1,
                Name = "Nico's First Playlist",
                Videos = new System.Collections.Generic.List<Video>()
                {
                    new Video() { Id = 1, Title = "Nico's First Video" },
                    new Video() { Id = 2, Title = "Nico's Second Video" }
                }
            };

            PlayLists playListsNico = new PlayLists()
            {
                PlayListMenu = new System.Collections.Generic.List<PlayList>() 
                {
                    playListNico
                }
            };

            SociaMediaChannel sociaMediaChannelNico = new SociaMediaChannel()
            {
                Owner = userNico,
                PlayLists = playListsNico,
            };

            int counter = 1;

            ///*----- Example 1 -----*///
            Console.WriteLine("///*----- Example {0} -----*///", counter);

            descriptionAttribute = sociaMediaChannelNico.GetDescriptionAttribute(nameof(sociaMediaChannelNico.PlayLists));

            infoText = descriptionAttribute.GetInfo(nameof(sociaMediaChannelNico.PlayLists));

            Console.WriteLine(infoText);
            Console.WriteLine();

            counter++;

            ///*----- Example 2 -----*///
            Console.WriteLine("///*----- Example {0} -----*///", counter);

            descriptionAttribute = sociaMediaChannelNico.GetDescriptionAttribute(nameof(sociaMediaChannelNico));

            infoText = descriptionAttribute.GetInfo(nameof(sociaMediaChannelNico));

            Console.WriteLine(infoText);
            Console.WriteLine();

            counter++;

            Console.WriteLine("End of {0} method call",MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod1 method call
///*----- Example 1 -----*///
About the property:PlayLists
        Description Info:
        PlayLists that category all videos
        Is the description applied on a default attribute?
        False


///*----- Example 2 -----*///
An `System.NullReferenceException` exception thrown at object.GetDescriptionAttribute extenstion method.
The error message:
Object reference not set to an instance of an object.
An `ArgumentNullException` exception thrown at descriptionAttribute.GetInfo extenstion method.
The error message:
Description attribute cannot be null. (Parameter 'descriptionAttribute')


End of TestMethod1 method call
```

## reference
### API docs
+ [`DescriptionAttribute Class`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.descriptionattribute?view=net-8.0)