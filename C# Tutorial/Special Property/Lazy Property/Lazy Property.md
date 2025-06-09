# Lazy Property
## What is Lazy property?
Object with Lazy property only initialized once. 

Object without Lazy property will be initialized once it is declared.

For example,

    void main()
    {
      for(int i=0;i<10;i++)
      {
        func();
      }
    }
    
    void func()
    {
        int x=3;
    }
    
In this example, x (in function func ) will be declared and initialized 10 times.

It is NOT neccessary, so we can mark x as lazy property.


## Syntax
    Lazy<Object> obj;
    
refers the obj is lazy.

That means the obj will be initialize once (at first time, the statement was executed).

## Ref
https://learn.microsoft.com/en-us/dotnet/framework/performance/lazy-initialization
  
