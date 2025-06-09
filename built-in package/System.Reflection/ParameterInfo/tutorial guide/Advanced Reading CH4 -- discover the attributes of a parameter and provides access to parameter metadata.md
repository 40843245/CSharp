# Advanced Reading CH4 -- discover the attributes of a parameter and provides access to parameter metadata
## objectives
You will learn how to

+ discover the attributes of a parameter and provides access to parameter metadata

## Advanced Reading CH4.1 -- discover the attributes of a parameter and provides access to parameter metadata
see following example.

## examples
### example 1
#### main code
This example shows how to use various reflection classes to analyze the metadata contained in an assembly.

Invoking following method

```
        /// <summary>
        /// illustrate how to discovers the attributes of a parameter and provides access to parameter metadata.
        /// </summary>
        public static void TestMethod1()
        {
            string fileFullPath = FilePathHandler.FileFullPathHandler.GetExportedMessageFullPath(MethodBase.GetCurrentMethod().Name);

            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

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
            AssemblyName[] referencedAssembliesName = assembly.GetReferencedAssemblies();
            foreach(AssemblyName referencedAssemblyName in referencedAssembliesName)
            {
                OutputHandler.Display(indentationLevel + 1 , "Name={0}, Version={1}, Culture={2}, PublicKey token={3}" , referencedAssemblyName.Name , referencedAssemblyName.Version , referencedAssemblyName.CultureInfo.Name , (BitConverter.ToString(referencedAssemblyName.GetPublicKeyToken())));
            }
            OutputHandler.Display(indentationLevel , "");

            Assembly[] currentAppDomainAssemblies = AppDomain.CurrentDomain.GetAssemblies();
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
                                    OutputHandler.Display(indentationLevel + 1 , "Parameter: Type={0}, Name={1}" , parameterInfo.ParameterType , parameterInfo.Name);
                                }
                            }

                            // If the member is a property, display information about the property's accessor methods.
                            if(memberInfos.MemberType == MemberTypes.Property)
                            {
                                MethodInfo [ ] accessorsInfo = ((PropertyInfo)memberInfos).GetAccessors();
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
+ [`ParameterInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0)

### examples
+ The code in example 1 is referenced from the first example in [`ParameterInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.parameterinfo?view=net-9.0) .Then it is modified and is refactored by author.

### further reading
+ About how to export output from console into a file (used in example 1), see

the code snippet generated by [Google Gemini Flash 2.5](https://g.co/gemini/share/80ca9e3862f7).
