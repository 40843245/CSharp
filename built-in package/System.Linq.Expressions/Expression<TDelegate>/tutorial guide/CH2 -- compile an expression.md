# CH2 -- compile an expression
## objectives
You will learn how to 

+ compile an expression

## CH2.1 -- compile an expression
Step 1:

Simply invoke the `Compile` instance method.

```
Expression<Func<int , bool>> expression1 = i => i < 5; // 
Func<int , bool> delegateFunction2 = expression1.Compile();
```

## examples
### example 1
Invoking following method 

```
        public static void TestMethod1()
        {
            Func<int , bool> delegateFunction1 = i => i < 5; // a delegate function.
           
            for(int i = 0; i < 10;i++) 
            {
                Console.WriteLine("delegateFunction1({0}) = {1}" , i , delegateFunction1(i));
            }

            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.
            Func<int , bool> delegateFunction2 = expression1.Compile(); // compile it, making it as an delegate function.

            Console.WriteLine();
            for(int i = 0; i < 10; i++)
            {
                Console.WriteLine("delegateFunction2({0}) = {1}" , i , delegateFunction2(i));
            }
        }
```

will output following

```
delegateFunction1(0) = True
delegateFunction1(1) = True
delegateFunction1(2) = True
delegateFunction1(3) = True
delegateFunction1(4) = True
delegateFunction1(5) = False
delegateFunction1(6) = False
delegateFunction1(7) = False
delegateFunction1(8) = False
delegateFunction1(9) = False

delegateFunction2(0) = True
delegateFunction2(1) = True
delegateFunction2(2) = True
delegateFunction2(3) = True
delegateFunction2(4) = True
delegateFunction2(5) = False
delegateFunction2(6) = False
delegateFunction2(7) = False
delegateFunction2(8) = False
delegateFunction2(9) = False
```

#### demo project
