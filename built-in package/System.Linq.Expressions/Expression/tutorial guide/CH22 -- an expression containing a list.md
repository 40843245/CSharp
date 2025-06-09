# CH22 -- an expression containing a list
## objectives
You wll learn

+ create an expression and initialize a list
+ create an expression that binds to a getter-setter property with `List<T>` type of the a class

## CH22.1 -- create an expression and initialize a list
### `Expression.ListInit` static method
creates a `ListInitExpression` that uses specified `ElementInit` objects to initialize a collection.

## CH22.2 -- create an expression that binds to a getter-setter property with `List<T>` type of the a class
### `Expression.ListBind` static method
creates a `MemberListBinding` object.

## examples
### exampl 1
Invokng following method

```
        /// <summary>
        /// illustrate how to use `Expression.ListInit`.
        /// </summary>
        public static void TestMethod36()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            
            // 1. Define the type of the list we want to create
            Type listType = typeof(List<string>);

            // 2. Get the constructor for List<string> (empty constructor)
            ConstructorInfo listConstructor = listType.GetConstructor(Type.EmptyTypes);

            // 3. Create a NewExpression representing the constructor call
            NewExpression newListaExpression = Expression.New(listConstructor);

            // 4. Get the MethodInfo for the Add method of List<string>
            MethodInfo addMethod = listType.GetMethod("Add" , new Type [ ] { typeof(string) });

            // 5. Create ElementInit objects for each item in the initializer list
            //    Each ElementInit needs the add method and the arguments (the string value)
            ElementInit init1 = Expression.ElementInit(addMethod , Expression.Constant("Apple"));
            ElementInit init2 = Expression.ElementInit(addMethod , Expression.Constant("Banana"));
            ElementInit init3 = Expression.ElementInit(addMethod , Expression.Constant("Cherry"));

            // 6. Create the ListInitExpression
            ListInitExpression listInit = Expression.ListInit(
                newListaExpression ,
                init1 ,
                init2 ,
                init3
            );

            // 7. Compile and execute the expression tree
            //    This creates a delegate that, when invoked, will perform the list initialization.
            Func<List<string>> createList = Expression.Lambda<Func<List<string>>>(listInit).Compile();

            List<string> myList = createList();

            Console.WriteLine("Created List:");
            foreach(string item in myList)
            {
                Console.WriteLine($"- {item}");
            }

            // You can also inspect the expression tree (optional)
            Console.WriteLine("\nExpression Tree (ListInit):");
            Console.WriteLine(listInit.ToString());
        }
```

will output following

```
In TestMethod36 method call,
Created List:
- Apple
- Banana
- Cherry

Expression Tree (ListInit):
new List`1() {Void Add(System.String)("Apple"), Void Add(System.String)("Banana"), Void Add(System.String)("Cherry")}
```

### example 2
Invokig the following method

```
/// <summary>
        /// illustrate how to use `Expression.ListBind`.
        /// </summary>
        public static void TestMethod37()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);
            // 1. Define the type of the Order class
            Type orderType = typeof(Order);

            // 2. Get MemberInfo for the 'Items' property
            PropertyInfo itemsProperty = orderType.GetProperty("Items");
            if(itemsProperty == null)
            {
                Console.WriteLine("Error: 'Items' property not found on Order class.");
                return;
            }

            // 3. Get the MethodInfo for the 'Add' method of List<string>
            MethodInfo addMethod = typeof(List<string>).GetMethod("Add" , new Type [ ] { typeof(string) });
            if(addMethod == null)
            {
                Console.WriteLine("Error: 'Add' method not found on List<string>.");
                return;
            }

            // 4. Create ElementInit objects for each item to be added to the 'Items' list
            ElementInit item1Init = Expression.ElementInit(addMethod , Expression.Constant("Laptop"));
            ElementInit item2Init = Expression.ElementInit(addMethod , Expression.Constant("Mouse"));

            // 5. Create the MemberListBinding for the 'Items' property
            // This represents the "Items = { ... }" part of the object initializer
            MemberListBinding itemsListBinding = Expression.ListBind(
                itemsProperty ,
                item1Init ,
                item2Init
            );

            // 6. Create MemberAssignment for other properties (like OrderId and CustomerName)
            // This represents "OrderId = 1" and "CustomerName = 'Alice'"
            MemberAssignment orderIdAssignment = Expression.Bind(
                orderType.GetProperty("OrderId") ,
                Expression.Constant(123)
            );

            MemberAssignment customerNameAssignment = Expression.Bind(
                orderType.GetProperty("CustomerName") ,
                Expression.Constant("Alice")
            );

            // 7. Create a NewExpression for the Order constructor (parameterless)
            NewExpression newOrder = Expression.New(orderType.GetConstructor(Type.EmptyTypes));

            // 8. Combine all bindings into a MemberInitExpression
            // This represents the entire object initializer: new Order { ... }
            MemberInitExpression orderInitializer = Expression.MemberInit(
                newOrder ,
                orderIdAssignment ,
                customerNameAssignment ,
                itemsListBinding // Add the ListBinding here
            );

            // 9. Compile and execute the expression tree
            Func<Order> createOrder = Expression.Lambda<Func<Order>>(orderInitializer).Compile();

            Order myOrder = createOrder();

            // 10. Verify the created object
            Console.WriteLine($"Order ID: {myOrder.OrderId}");
            Console.WriteLine($"Customer Name: {myOrder.CustomerName}");
            Console.WriteLine("Order Items:");
            foreach(var item in myOrder.Items)
            {
                Console.WriteLine($"- {item}");
            }

            Console.WriteLine("\nExpression Tree (MemberInit with ListBind):");
            Console.WriteLine(orderInitializer.ToString());
        }
```

where 

`Order` class is defined as follows.

`Order.cs`

```
using System.Collections.Generic;

namespace Example.Bean
{
    public class Order
    {
        public int OrderId { get; set; }
        public List<string> Items { get; set; } = new List<string>(); // Ensure it's initialized
        public string CustomerName { get; set; }
    }
}
```

It will output following

```
In TestMethod37 method call,
Order ID: 123
Customer Name: Alice
Order Items:
- Laptop
- Mouse

Expression Tree (MemberInit with ListBind):
new Order() {OrderId = 123, CustomerName = "Alice", Items = {Void Add(System.String)("Laptop"), Void Add(System.String)("Mouse")}}
```

## reference
### API docs
+ [`Expression.ListInit Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.listinit?view=net-8.0)
+ [`Expression.ListBind Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.listbind?view=net-8.0)
+ [`ListInitExpression Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.listinitexpression?view=net-8.0)
+ [`MemberListBinding Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.memberlistbinding?view=net-8.0)