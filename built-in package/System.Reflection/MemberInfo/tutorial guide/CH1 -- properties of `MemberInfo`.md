# CH1 -- properties of `MemberInfo`
## objectives
You will know these common properties of `MemberInfo`.

## CH1.1 -- properties of `MemberInfo`
+ [`MemberInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0)

## examples
### example 1
#### main code
In this example, it will list some metadata about assembly of class the root project `Program.cs`.

> [!NOTE]
> For new babie, to know the meaning of properties of `MemberInfo` instance.
>
> Just pay attention to this code block.
> 
> ```
> MemberInfo [ ] memberInfos = currentAppDomainExportedType.GetMembers();
> foreach(MemberInfo memberInfo in memberInfos){
>    
> }
> ```
>
> Other parts of code snippet is unrelated to `MemberInfo` class.
>
> and it is harder to understand, 
>
> even one is familar with reflection and the use case of class under `System.Reflection` namespace.
>
> Thus, I NOT recommend to pay lots of attention to other parts.
>

> [!NOTE]
> The code snippet is similar to code snippet of example 1 in [Advanced Reading CH3 -- discover the attributes of a parameter and provides access to parameter metadata.md](../../ParameterInfo/tutorial%20guide/Advanced%20Reading%20CH3%20--%20discover%20the%20attributes%20of%20a%20parameter%20and%20provides%20access%20to%20parameter%20metadata.md)
> 
> which is referenced from the first example in MSDS [`MemberInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo?view=net-9.0) and then is modified by author.

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
            Console.WriteLine("You can see these exported output at `{0}` file" , fileFullPath);
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

                // Display the set of assemblies our assemblies reference.

                OutputHandler.Display(indentationLevel , "Referenced assemblies:");
                AssemblyName [ ] referencedAssembliesName = assembly.GetReferencedAssemblies();
                foreach(AssemblyName referencedAssemblyName in referencedAssembliesName)
                {
                    OutputHandler.Display(indentationLevel + 1 , "Name={0}, Version={1}, Culture={2}, PublicKey token={3}" , referencedAssemblyName.Name , referencedAssemblyName.Version , referencedAssemblyName.CultureInfo.Name , (BitConverter.ToString(referencedAssemblyName.GetPublicKeyToken())));
                }
                OutputHandler.Display(indentationLevel , "");

                Assembly [ ] currentAppDomainAssemblies = AppDomain.CurrentDomain.GetAssemblies();
                // Display information about each assembly loading into this AppDomain.
                foreach(Assembly currentAppDomainAssembly in currentAppDomainAssemblies)
                {
                    OutputHandler.Display(indentationLevel , "Assembly: {0}" , currentAppDomainAssembly);

                    Module [ ] currentAppDomainModules = currentAppDomainAssembly.GetModules(true);
                    // Display information about each module of this assembly.
                    foreach(Module currentAppDomainModule in currentAppDomainModules)
                    {
                        OutputHandler.Display(indentationLevel + 1 , "Module: {0}" , currentAppDomainModule.Name);
                    }

                    // Display information about each type exported from this assembly.
                    Type [ ] currentAppDomainExportedTypes = currentAppDomainAssembly.GetExportedTypes();
                    indentationLevel += 1;
                    foreach(Type currentAppDomainExportedType in currentAppDomainExportedTypes)
                    {
                        OutputHandler.Display(0 , "");
                        OutputHandler.Display(indentationLevel , "Type: {0}" , currentAppDomainExportedType);

                        // For each type, show its members & their custom attributes
                        indentationLevel += 1;
                            MemberInfo [ ] memberInfos = currentAppDomainExportedType.GetMembers();

                            foreach(MemberInfo memberInfo in memberInfos)
                            {
                            string separator = "+";
                            string newLine = System.Environment.NewLine;
                            string tab = "\t";
                            string space = " ";

                            string formattingString = string.Empty;

                            formattingString += "{1}{2}{0}{3}Name={4},";
                            formattingString += "{1}{2}{0}{3}DeclaringType.FullName={5},";
                            formattingString += "{1}{2}{0}{3}ReflectedType.FullName={6},";
                            formattingString += "{1}{2}{0}{3}MemberType={7},";
                            formattingString += "{1}{2}{0}{3}Is defined in which Module? {8},";
                            formattingString += "{1}{2}{0}{3}MetadataToken={9},";

                            /// Invoke `OutputHandler.DisplayWithColor` if you want to
                            /// display text with different color
                            /// OutputHandler.DisplayWithColor(
                            OutputHandler.Display(
                                indentationLevel,
                                //ConsoleColor.Green,
                                //ConsoleColor.Black,
                                formattingString,
                                separator,
                                newLine,
                                tab,
                                space,
                                memberInfo.Name,
                                memberInfo.DeclaringType.Name,
                                memberInfo.ReflectedType.Name,
                                memberInfo.MemberType,
                                memberInfo.Module.Name,
                                memberInfo.MetadataToken
                                );
                            // output its custom attributes
                            OutputHandler.DisplayAttributes(indentationLevel , memberInfo);

                            // If the member is a method, display information about its parameters.

                            if(memberInfo.MemberType == MemberTypes.Method)
                            {
                                ParameterInfo [ ] parameterInfos = ((MethodInfo)memberInfo).GetParameters();
                                foreach(ParameterInfo parameterInfo in parameterInfos)
                                {
                                    OutputHandler.Display(indentationLevel + 1 , "Parameter: Type={0}, Name={1}" , parameterInfo.ParameterType , parameterInfo.Name);
                                }
                            }

                            // If the member is a property, display information about the property's accessor methods.
                            if(memberInfo.MemberType == MemberTypes.Property)
                            {
                                MethodInfo [ ] accessorsInfo = ((PropertyInfo)memberInfo).GetAccessors();
                                foreach(MethodInfo accessorInfo in accessorsInfo)
                                {
                                    OutputHandler.Display(indentationLevel + 1 , "Accessor method: {0}" , accessorInfo);
                                }
                            }
                        }
                        indentationLevel -= 1;
                    }
                    indentationLevel -= 1;
                }
            }

            Console.SetOut(originalConsoleOut);

            Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
        }
```

will export these info into a log (`.log`) file under `~/AppData/Output/Console` directory

after executing this method.

## reference
### API docs
+ [`MemberInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo?view=net-9.0)

### examples
+ The code in example 1 is referenced from the first example in [`MemberInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.memberinfo?view=net-9.0) .Then it is modified and is refactored by author.

### further reading
+ About how to export output from console into a file (used in example 1), see

the code snippet generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/80ca9e3862f7).