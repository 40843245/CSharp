# Requirements of current projects
To use syntax in `C#` which supports in `C#` 11.0.

+ [`.NET: 9.0.0`](https://dotnet.microsoft.com/en-us/download/dotnet/9.0) (or above)

+ `MSBuild`: For projects with `.NET: 9.0.0`, please use the latest version of `MSBuild`.

The most user-friendly way is to download [`Visual Studio 2022`](Visual Studio 2022) (or above)

![url of vs-community 2022 download.png](url%20of%20vs-community%202022%20download.png)

# Migration of current projects
If your project uses `.NET` older than `9.0.0` version, you have to migrate it to `.NET 9.0.0` or above.

1. Before migrating, please

+ download `.NET 9.0.0` or above (if you have NOT download yet), then upgrade to `.NET 9.0.0` or above.

After upgrading to `.NET 9.0.0` or above, please

+ download the latest version of `MSBuild` (if you have NOT download yet).

2. To migrate the project,

+ (Recommend) Use the most-friendly and less troublesome approach -- [`upgrade assistant`](https://dotnet.microsoft.com/en-us/platform/upgrade-assistant) package in CLI.

For more information, see my notes [upgrade assistant tool in CLI.md](https://github.com/40843245/Visual-Studio/blob/main/tools/upgrade%20assistant/upgrade%20assistant%20tool%20in%20CLI.md)

