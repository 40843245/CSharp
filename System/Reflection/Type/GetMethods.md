# GetMethods in C#
## Info
### Assembly
System.Runtime.dll
### Namespace
System
## Introduction
Gets all methods of the current Type filtered by specified attributes.
## Overload
    GetMethods(BindingFlags)
    GetMethods()
### GetMethods(BindingFlags)
#### Syntax
    public abstract System.Reflection.MethodInfo[] 
    GetMethods 
    (System.Reflection.BindingFlags bindingAttr);
#### Introduction 
        See the Parameter section.
#### Parameter
       bindingAttr: 
            To filter the results by one or more attributes as the flag. You can consider it a flag.
#### Return type
       System.Reflection.MethodInfo[] 
#### Return value
        It returns a resultant array that contain all available methods of given class if exists. Otherwise, return null array (i.e. array of 0 elements).
        The type of resultant array should be either null or the System.Reflection.MethodInfo class which contains infos about the available methods.
### GetMethods()
#### Syntax
    public abstract System.Reflection.MethodInfo[] 
    GetMethods 
    ();
#### Introduction 
        When the flag is NOT specified, it assumes that one search all public methods.
#### Parameter
None        
#### Return type
       System.Reflection.MethodInfo[] 
#### Return value
        It returns a resultant array that contain all public methods of given class. Otherwise, return null array (i.e. array of 0 elements).
        The type of resultant array should be either null or the System.Reflection.MethodInfo class which contains infos about the available methods.
## Application
### List All Methods in C#
#### Example 
##### Example 1
###### Code 1

    void Init()
    {
        ListAllMethods(typeof(DateTime));
    }
    void ListAllMethods(Type type)
    {
        var methods=type.GetMethods();
        foreach (var method in methods)
        {
            var parameters = method.GetParameters();
            Debug.LogWarningFormat("{0} {1}",
                              method.ReturnType,
                              method.Name
                              );
        }
    }
   
###### Result

Screenshot that illustrate the result of code in Unity Project.

![image](https://github.com/40843245/CSharp/assets/75050655/b455e665-376c-4676-87b4-51d3af963dd1)

![image](https://github.com/40843245/CSharp/assets/75050655/f0f6d74a-e89e-4018-965a-4d3aeb5299c1)

![image](https://github.com/40843245/CSharp/assets/75050655/3c3302d4-5462-4bfb-acb7-7446f31892f9)

![image](https://github.com/40843245/CSharp/assets/75050655/f54e2827-9fec-4435-942b-70e54dbdd56e)

etc

#### Ref 

For the issue, list all available methods of class in C#.

![image](https://github.com/40843245/CSharp/assets/75050655/3318291d-5203-422f-8308-acd63f77f3cc)

Thanks to the reply on StackOverflow.

![image](https://github.com/40843245/CSharp/assets/75050655/d2227e7d-1989-4dd8-a5cd-8304f4af46b2)

https://stackoverflow.com/questions/1198417/generate-list-of-methods-of-a-class-with-method-types

Here is the API of MSDS (Microsoft Docs).

https://learn.microsoft.com/en-us/dotnet/api/system.type.getmethods?view=net-7.0

