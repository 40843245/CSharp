# CH2 -- compile an expression
## objectives
You will learn how to 

+ compile an expression

**extra bonus**

You may wonder that these following are referentially equal and non-referentially equal.

+ two delegate functions made in different way

  - first delegate function is directly initialized.
  - second delegate function is made from an expression in form of an expression tree.

+ two delegate functions are made from an expression in form of an expression tree but with different `Compile` instance method call.

## CH2.1 -- compile an expression
Step 1:

Simply invoke the `Compile` instance method.

```
Expression<Func<int , bool>> expression1 = i => i < 5; // 
Func<int , bool> delegateFunction2 = expression1.Compile();
```

## CH2.2 -- referentially equal and non-referentially equal issues
You may wonder that these following are referentially equal and non-referentially equal.

+ two delegate functions made in different way

  - first delegate function is directly initialized.
  - second delegate function is made from an expression in form of an expression tree.

+ two delegate functions are made from an expression in form of an expression tree but with different `Compile` instance method call.

I will discuss it then use examples (example 2 and example 3) to prove it that there are **NEITHER** referentially equal **NOR** non-referentially equal.

Lots of people thinks that

> If two delegates have same behavior, they MUST ~~non-referentially equal~~.
>
> However, it is **wrong**.

example 2 and example 3 will prove it.

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

> [!IMPORTANT]
> Although these two delegate functions do same behavior,
>
> there are not only **NOT** **referientially equal** (of course, as one delegate function does NOT points to another delegate function)
>
> but also **NOT** **non-referientially equal**.
>
> see example 2 and example 3 for more understanding.

#### demo project

### example 2
Invoking following method 

```
        /// <summary>
        /// Compare two delegate function are referentially equal.
        /// 
        /// First delegate function is directly initialized.
        /// 
        /// While, second delegate function is made from an expression in form of an expression tree.
        /// </summary>
        public static void TestMethod2()
        {
            Func<int , bool> delegateFunction1 = i => i < 5; // a delegate function.

            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.
            Func<int , bool> delegateFunction2 = expression1.Compile(); // compile it, making it as an delegate function.

            bool isEqual = object.Equals(delegateFunction1 , delegateFunction2);
            bool isReferentiallyEqual = object.ReferenceEquals(delegateFunction1,delegateFunction2);
            Console.WriteLine($"Are these delegate functions equal? {isEqual}");

            Console.WriteLine($"Are these delegate functions referentially equal? {isReferentiallyEqual}");
        }
```

will output following

```
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
```

In this example, we can prove that

> there are not only **NOT** **referientially equal** (using `object.ReferenceEqual` static method call)
>
> but also **NOT** **non-referientially equal** (using `object.Equals` static method call)

#### demo project

### example 3
Invoking following method

```
        /// <summary>
        /// Compare two delegate function are referentially equal.
        /// 
        /// First delegate function and second delegate function is made from an expression in form of an expression tree. But made with different `Compile` instance method call. 
        /// </summary>
        public static void TestMethod3()
        {
            Expression<Func<int , bool>> expression1 = i => i < 5; // represents a delegate function in form of an expression tree.

            Func<int , bool> delegateFunction1 = expression1.Compile(); // compile it, making it as an delegate function.
            Func<int , bool> delegateFunction2 = expression1.Compile(); // compile it, making it as an delegate function.

            bool isEqual = object.Equals(delegateFunction1 , delegateFunction2);
            bool isReferentiallyEqual = object.ReferenceEquals(delegateFunction1 , delegateFunction2);
            Console.WriteLine($"Are these delegate functions equal? {isEqual}");

            Console.WriteLine($"Are these delegate functions referentially equal? {isReferentiallyEqual}");
        }
```

will output following

```
Are these delegate functions equal? False
Are these delegate functions referentially equal? False
```

#### demo project
