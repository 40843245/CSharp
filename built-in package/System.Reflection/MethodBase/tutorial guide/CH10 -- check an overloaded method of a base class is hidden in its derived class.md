# CH10 -- check an overloaded method of a base class is hidden in its derived class
## objectives
You will learn how to

+ check an overloaded method of a base class is hidden in its derived class

## CH10.1 -- check an overloaded method of a base class is hidden in its derived class
### `IsHideBySig` property

check an overloaded method of a base class is hidden in its derived class by `new` keyword to an overloaded method in its derived class

> [!NOTE]
> `new` keyword in an overloaded method defined in its derived class is used to hide the overloaded method defined in based class.

## examples
### example 1
#### Bean
see demo project.

#### main code
Invoking following method

```
```

will output following 

```
In TestMethod3 method call,
------ List the overloads of PlaceOrder in the derived class Example.Behaviors.Restaurant.BuffetRestuarant ------
Overloaded method PlaceOrder: Void PlaceOrder(Example.Beans.Order)  IsHideBySig = True, DeclaringType = Example.Behaviors.Restaurant.RestaurantBase
Overloaded method PlaceOrder: Void PlaceOrder()  IsHideBySig = True, DeclaringType = Example.Behaviors.Restaurant.RestaurantBase
------ Call the overloads of PlaceOrder available in derived class Example.Behaviors.Restaurant.BuffetRestuarant ------
Welcome to Buffet Paradise Buffet Restuarantat Las Vegas!
You will be able to enjoy the these meal:
- Pizza
- Pasta
Welcome to Buffet Paradise Buffet Restuarantat Las Vegas!
You will be able to enjoy the these meal:
- strawberry milk
Welcome to Buffet Paradise Restaurant at Las Vegas!
Please place your order at the counter.
------ Call the overloads of PlaceOrder available in base class Example.Behaviors.Restaurant.RestaurantBase ------
Yazawa Nico places order at Las Vegas.
Order information:
Item ID: 1, Product: Pizza, Price: 9.99
Item ID: 2, Product: Pasta, Price: 7.99
Ai places order at Las Vegas.
Order information:
Item ID: 1, Product: strawberry milk, Price: 9
Welcome to Buffet Paradise Restaurant at Las Vegas!
Please place your order at the counter.
------------ Order List in Buffet Paradise, which is a Buffet Restuarant --------------------
Orders at Buffet Paradise Restaurant at Las Vegas:
Order ID: 1
Customer Name: Yazawa Nico
There are 2 items in this order:
Info about 1th Item:
        Item ID: 1, Product: Pizza, Price: 9.99
Info about 2th Item:
        Item ID: 2, Product: Pasta, Price: 7.99

Order ID: 2
Customer Name: Ai
There are 1 items in this order:
Info about 1th Item:
        Item ID: 1, Product: strawberry milk, Price: 9

Order ID: 1
Customer Name: Yazawa Nico
There are 2 items in this order:
Info about 1th Item:
        Item ID: 1, Product: Pizza, Price: 9.99
Info about 2th Item:
        Item ID: 2, Product: Pasta, Price: 7.99

Order ID: 2
Customer Name: Ai
There are 1 items in this order:
Info about 1th Item:
        Item ID: 1, Product: strawberry milk, Price: 9

------------ Order List in Buffet Paradise, which is a Buffet Restuarant --------------------
Orders at Buffet Paradise Restaurant at Las Vegas:
Order ID: 1
Customer Name: Yazawa Nico
There are 2 items in this order:
Info about 1th Item:
        Item ID: 1, Product: Pizza, Price: 9.99
Info about 2th Item:
        Item ID: 2, Product: Pasta, Price: 7.99

Order ID: 2
Customer Name: Ai
There are 1 items in this order:
Info about 1th Item:
        Item ID: 1, Product: strawberry milk, Price: 9

Order ID: 1
Customer Name: Yazawa Nico
There are 2 items in this order:
Info about 1th Item:
        Item ID: 1, Product: Pizza, Price: 9.99
Info about 2th Item:
        Item ID: 2, Product: Pasta, Price: 7.99

Order ID: 2
Customer Name: Ai
There are 1 items in this order:
Info about 1th Item:
        Item ID: 1, Product: strawberry milk, Price: 9

End of TestMethod3 method call,
```

To see highlighted output in Console, see [output of TestMethod3 method call.docx](output%20of%20TestMethod3%20method%20call.docx)

## reference
### API docs
+ [`MethodBase.IsHideBySig Property`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.ishidebysig?view=net-9.0)