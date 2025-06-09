# Advanced Reading CH1 -- summary
## objectives
Summarize all knowledgement you have learned before with an example.

## examples
### example 1
#### extension method
see demo project.
#### main code
Invoking following method

```
       /// <summary>
       /// illustrate 
       /// 
       /// + how to get current method info 
       /// 
       /// using reflection 
       /// 
       /// (by invoking `MethodBase.GetCurrentMethod()`)
       /// 
       /// + properties and methods in `MethodBase` class.
       /// </summary>
       public static void TestMethod4()
       {
           Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

           string infoText = string.Empty;
           
           infoText = MethodBase.GetCurrentMethod().GetInfo();

           Console.WriteLine(infoText);

           Console.WriteLine("End of {0} method call," , MethodBase.GetCurrentMethod().Name);
       }
```

will output following

```
In TestMethod4 method call,
Current Method Name: TestMethod4
Declaring Type: Example.DemoClass.DemoClass1
Member Type: Method
Reflected Type: Example.DemoClass.DemoClass1
Return Type: System.Void
Is Static: True
Is Virtual: False
Is Public: True
Is Private: False
Is Family: False
Is Assembly: False
Is Constructor: False
Is Generic Method: False
Is Generic Method Definition: False
Is Abstract: False
Is Final: False
Is HideBySig: True
Is SpecialName: False
Is SecurityCritical: True
Is SecuritySafeCritical: False
Is SecurityTransparent: False
Is Defined: False
Method Handle: System.RuntimeMethodHandle
Calling Convention: Standard
Metadata Token: 100663347
Module: Example.dll
Get Method Implementation Flags by accessing `MethodImplementationFlags`: IL
Get Method Implementation Flags by invoking: `GetMethodImplementationFlags()` instance method: ILAttributes: Public, Static, HideBySig
Get Parameters:

Get Generic Arguments:

Get Method Body:
  + Local Variables Count: 1
  + Max Stack Size: 2
    + Local Variable Type: System.String
    + Local Variable Index: 0
    + IsPinned: False


Get Custom Attributes:

Get Custom Attributes (Obsolete):

Get Custom Attributes (Obsolete):


End of TestMethod4 method call,
```