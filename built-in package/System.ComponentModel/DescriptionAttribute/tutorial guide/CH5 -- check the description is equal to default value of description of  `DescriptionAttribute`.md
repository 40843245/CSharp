# CH5 -- check the description is equal to default value of description of  `DescriptionAttribute`
## objectives
You will learn how to

+ check the description is equal to default value of description of  `DescriptionAttribute`

## CH5.1 -- check the description is equal to default value of description of `DescriptionAttribute`
### `IsDefaultAttribute` instance method
It seems that 

+ `IsDefaultAttribute` instance method of current `DescriptionAttribute` instance will compare the description of current `DescriptionAttribute` instance is equal to the description of `DescriptionAttribute.Default` (which evaluates to empty string).

The following example can prove it.

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate how to check the `DescriptionAttribute` instance is a default value or not
        /// by invoking `DescriptionAttribute.IsDefaultAttribute()` instance method.
        /// </summary>
        public static void TestMethod3()
        {
            Console.WriteLine("In {0} method call", MethodBase.GetCurrentMethod().Name);

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

            descriptionAttribute = sociaMediaChannelNico.GetDescriptionAttribute(nameof(sociaMediaChannelNico.PlayLists));

            Console.WriteLine("Is it a default attribute?`{0}`", descriptionAttribute.IsDefaultAttribute().ToString());

            descriptionAttribute = userNico.GetDescriptionAttribute(nameof(userNico.Name));

            Console.WriteLine("Is it a default attribute?`{0}`", descriptionAttribute.IsDefaultAttribute().ToString());

            Console.WriteLine("Is DescriptionAttribute.Default a default attribute?`{0}`", DescriptionAttribute.Default.IsDefaultAttribute().ToString());
            Console.WriteLine("End of {0} method call", MethodBase.GetCurrentMethod().Name);
        }
```

will output following

```
In TestMethod3 method call
Is it a default attribute?`False`
Is it a default attribute?`True`
Is DescriptionAttribute.Default a default attribute?`True`
End of TestMethod3 method call
```

## reference
### API docs
+ [`DescriptionAttribute.IsDefaultAttribute Method`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.descriptionattribute.isdefaultattribute?view=net-8.0)