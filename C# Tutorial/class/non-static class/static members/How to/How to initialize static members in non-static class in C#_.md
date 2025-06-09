# How to initialize static members in non-static class in C#?
## Answer
Defining a private static constructor in non-static class. 

See following code snippets.

```
public class NonStaticClass
{
  private int counter;
  private static NonStaticClass()
  {
    this.counter = 0;
  }
}
```
