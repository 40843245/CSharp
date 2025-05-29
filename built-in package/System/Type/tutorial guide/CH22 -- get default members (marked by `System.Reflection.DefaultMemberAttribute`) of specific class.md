# CH22 -- get default members (marked by `System.Reflection.DefaultMemberAttribute`) of specific class
## objectives
You will learn how to

+ get default members (marked by `System.Reflection.DefaultMemberAttribute`) of specific class

## CH22.1 -- get default members (marked by `System.Reflection.DefaultMemberAttribute`) of specific class
### `GetDefaultMembers` intstance method
`GetDefaultMembers` intstance method will search all default members 

(exactly said, members that are marked with `System.Reflection.DefaultMemberAttribute` (which located in class definition))

see following example for more understanding.

> [!TIP]
> All annotations about attribute can be shorten to `XXX` from `<XXX>Attribute` in `C#`.
>
> Applying this principle, `[DefaultMember("Lowerbound")]` is shortened from `[DefaultMemberAttribute("Lowerbound")]`.
## examples
### example 1
#### Bean
`Volume` class

```
using System.Reflection;

namespace Example.Bean
{
    [DefaultMember("Lowerbound")]
    public class Volume
    {
        public int Lowerbound { get; set; }
        public int Upperbound { get; set; }
        public int Loudness { get; set; }
    }
}
```

#### main code
Invoke following method

```
        /// <summary>
        /// illustrate how to search default members (exactly said, members that are marked with
        /// 
        /// `System.Reflection.DefaultMemberAttribute` (which located in class definition))
        /// </summary>
        public static void TestMethod25()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;

            MemberInfo [ ] memberInfos;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            Type volumeType = typeof(Volume);

            memberInfos = volumeType.GetDefaultMembers();

            if(memberInfos.Length > 0)
            {
                foreach(MemberInfo memberInfo in memberInfos)
                {
                    Console.WriteLine("The default member name is: " + memberInfo.ToString());
                }
            }
            else
            {
                Console.WriteLine("No default members are available.");
            }

            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod25 method call
///* ------------------ Example 1 ------------------ *///
The default member name is: Int32 Lowerbound

```

## reference
### API docs
+ [`Type.GetDefaultMembers Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getdefaultmembers?view=net-8.0)

+ [`DefaultMemberAttribute Class`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getdefaultmembers?view=net-8.0)