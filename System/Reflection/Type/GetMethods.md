# GetMethods in C#
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

![Uploading image.png…]()

