# CH1 -- properties of `ParameterInfo`
## objectives
You will know these common properties of `ParameterInfo`.

## CH1.1 -- properties of `ParameterInfo`
+ [`ParameterInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0)

## examples
### example 1
#### main code
In this example, it will list some metadata about assembly of class the root project `Program.cs`.

> [!NOTE]
> For new babie, to know the meaning of properties of `ParameterInfo` instance.
>
> Just pay attention to this code block.
> 
> ```
> if(memberInfos.MemberType == MemberTypes.Method)
> ```
>
> Other parts of code snippet is unrelated to `ParameterInfo` class.
>
> and it is harder to understand, 
>
> even one is familar with reflection and the use case of class under `System.Reflection` namespace.
>
> Thus, I NOT recommend to pay lots of attention to other parts.
>

> [!NOTE]
> The code snippet is similar to code snippet of example 1 in [Advanced Reading CH3 -- discover the attributes of a parameter and provides access to parameter metadata.md](Advanced%20Reading%20CH3%20--%20discover%20the%20attributes%20of%20a%20parameter%20and%20provides%20access%20to%20parameter%20metadata.md)
> 
> which is referenced from the first example in MSDS [`ParameterInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0) and then is modified by author.

Invoking following method

```
        /// <summary>
        /// list some infos about assembly of class the root project `Program.cs`.
        /// 
        /// Export these infos into a log (`.log`) file under `~/AppData/Output/Console` directory.
        /// </summary>
        public static void TestMethod2()
        {
            string fileFullPath = FilePathHandler.FileFullPathHandler.GetExportedMessageFullPath(MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            Console.WriteLine("The output on the console will be exported into a `.log` file.");
            Console.WriteLine("Press the key after it finishes will see output on the console that is exported into the `.log` file");
            Console.WriteLine("Please open the log file until the it finishes to write output into the log file.\nOtherwise, you will see incomplete output");
            Console.WriteLine("You can see these exported output at `{0}` file",fileFullPath);
            Console.WriteLine("Waiting a while...");

            TextWriter originalConsoleOut = Console.Out;

            using(
                StreamWriter consoleStreamWriter =
                new StreamWriter(fileFullPath , true , Encoding.UTF8)
            )
            {
                Console.SetOut(consoleStreamWriter);

                // This variable holds the amount of indenting that
                // should be used when displaying each line of information.
                int indentationLevel = 0;
                // Display information about the EXE assembly about `Example.Program`.
                Assembly assembly = typeof(Example.Program).Assembly;
                OutputHandler.Display(indentationLevel , "Assembly identity={0}" , assembly.FullName);
                OutputHandler.Display(indentationLevel + 1 , "Codebase={0}" , assembly.CodeBase);

                Assembly [ ] currentAppDomainAssemblies = AppDomain.CurrentDomain.GetAssemblies();
                // Display information about each assembly loading into this AppDomain.
                foreach(Assembly currentAppDomainAssembly in currentAppDomainAssemblies)
                {
                    OutputHandler.Display(indentationLevel , "Assembly: {0}" , currentAppDomainAssembly);

                    // Display information about each type exported from this assembly.
                    Type [ ] currentAppDomainExportedTypes = currentAppDomainAssembly.GetExportedTypes();
                    indentationLevel += 1;
                    foreach(Type currentAppDomainExportedType in currentAppDomainExportedTypes)
                    {
                        OutputHandler.Display(0 , "");
                        OutputHandler.Display(indentationLevel , "Type: {0}" , currentAppDomainExportedType);

                        // For each type, show its members & their custom attributes.

                        indentationLevel += 1;
                        foreach(MemberInfo memberInfos in currentAppDomainExportedType.GetMembers())
                        {
                            OutputHandler.Display(indentationLevel , "Member: {0}" , memberInfos.Name);
                            OutputHandler.DisplayAttributes(indentationLevel , memberInfos);

                            // If the member is a method, display information about its parameters.

                            if(memberInfos.MemberType == MemberTypes.Method)
                            {

                                ParameterInfo [ ] parameterInfos = ((MethodInfo)memberInfos).GetParameters();
                                foreach(ParameterInfo parameterInfo in parameterInfos)
                                {
                                    string separator = "+";
                                    string newLine = System.Environment.NewLine;
                                    string tab = "\t";
                                    string space = " ";

                                    string defaultValue = parameterInfo?.DefaultValue?.ToString() ?? "null";

                                    string rawDefaultValue = parameterInfo?.RawDefaultValue?.ToString() ?? "null";

                                    MemberInfo parameterInfoMemberInfo = parameterInfo.Member;

                                    string parameterInfoMemberInfoName = parameterInfoMemberInfo?.Name ?? "null";

                                    string formattingString = string.Empty;

                                    formattingString += "{1}{2}{0}{3}ParameterType.FullName={4},";
                                    formattingString += "{1}{2}{0}{3}Type={5},";
                                    formattingString += "{1}{2}{0}{3}Name={6},";
                                    formattingString += "{1}{2}{0}{3}Attributes={7},";
                                    formattingString += "{1}{2}{0}{3}HasDefaultValue={8},";
                                    formattingString += "{1}{2}{0}{3}DefaultValue={9},";
                                    formattingString += "{1}{2}{0}{3}RawDefaultValue={10},";
                                    formattingString += "{1}{2}{0}{3}IsIn (Is Input)={11},";
                                    formattingString += "{1}{2}{0}{3}IsOut (Is Output)={12},";
                                    formattingString += "{1}{2}{0}{3}IsLcid (Is Locale Identifier)={13},";
                                    formattingString += "{1}{2}{0}{3}IsRetval={14},";
                                    formattingString += "{1}{2}{0}{3}IsOptional={15},";
                                    formattingString += "{1}{2}{0}{3}IsRetval (Is return value type)={16},";
                                    formattingString += "{1}{2}{0}{3}Member's Name={17},";
                                    formattingString += "{1}{2}{0}{3}MetadataToken={18},";


                                    /// Invoke `OutputHandler.DisplayWithColor` if you want to display highlighted text with different color on Console.
                                    OutputHandler.Display(
                                    indentationLevel + 1 ,
                                        // ConsoleColor.Green ,
                                        // ConsoleColor.Black,
                                        formattingString ,
                                        separator ,
                                        newLine ,
                                        tab ,
                                        space ,
                                        parameterInfo.ParameterType.FullName ,
                                        parameterInfo.Name ,
                                        parameterInfo.Attributes ,
                                        parameterInfo.HasDefaultValue ,
                                        (defaultValue.Equals(string.Empty) == false) ? defaultValue : "`empty string`" ,
                                        (rawDefaultValue.Equals(string.Empty) == false) ? defaultValue : "`empty string`" ,
                                        parameterInfo.IsIn ,
                                        parameterInfo.IsOut ,
                                        parameterInfo.IsLcid ,
                                        parameterInfo.IsRetval ,
                                        parameterInfo.IsOptional ,
                                        parameterInfo.Position ,
                                        (parameterInfoMemberInfoName.Equals(string.Empty) == false) ? parameterInfoMemberInfoName : "`empty string`",
                                        parameterInfo.Member.Name,
                                        parameterInfo.MetadataToken
                                    );

                                    IEnumerable<CustomAttributeData> customAttributeDatas =
                                        parameterInfo.CustomAttributes;
                                    foreach(CustomAttributeData customAttributeData in customAttributeDatas)
                                    {
                                        OutputHandler.Display(
                                            indentationLevel + 2 ,
                                            "CustomAttributeData: AttributeType.FullName={0}" ,
                                            customAttributeData.AttributeType.FullName
                                        );
                                    }
                                }
                            }
                        }
                        indentationLevel -= 1;
                    }
                    indentationLevel -= 1;
                }
                consoleStreamWriter.Flush();
            }

            Console.SetOut(originalConsoleOut);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will export these info into a log (`.log`) file under `~/AppData/Output/Console` directory

after executing this method.

## reference
### API docs
+ [`ParameterInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0)

### examples
+ The code in example 1 is referenced from the first example in [`ParameterInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0) .Then it is modified and is refactored by author.

### further reading
+ About how to export output from console into a file (used in example 1), see

the code snippet generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/80ca9e3862f7).