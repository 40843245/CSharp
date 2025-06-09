# CH29 -- dispatch the specific `NodeType` of an expression
## objectives
You will learn how to

+ dispatch the specific `NodeType` of an expression

## CH29.1 -- dispatch the specific `NodeType` of an expression

step 1:
 
define a custom visitor (a class) that inherits `ExpressionVisitor`

step 2:

then implements its methods.

here, we need to implement at least these methods:

+ `Visit`: behavior of visiting the expression tree. 

step 3:

instantiate the custom visitor.

```
ExpressionLogger logger = new ExpressionLogger();
```

step 4:

define an expression tree.

```
Expression<Func<int , int , bool>> filter = (x , y) => (x + y) * 2 > 10 && x < 100;
```

step 5:

visit the expression tree by invoking `Visit` method of the custom visitor.

```
logger.Visit(filter); // ExpressionVisitor's Visit method internally uses Accept
```

## examples
### example 1
Invoking following method

```
        /// <summary>
        /// illustrate the usage of `Expression.Accept`
        /// </summary>
        public static void TestMethod49()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            // Define a simple expression tree
            Expression<Func<int , int , bool>> filter = (x , y) => (x + y) * 2 > 10 && x < 100;

            Console.WriteLine("Original Expression:");
            Console.WriteLine(filter.ToString());
            Console.WriteLine("\n--- Visiting Expression Tree ---");

            // Create an instance of our custom visitor
            var logger = new ExpressionLogger();

            // The core of the example: Call Accept on the root expression
            // This initiates the traversal process, where the Accept method
            // on each expression node calls the appropriate VisitX method on the visitor.
            logger.Visit(filter); // ExpressionVisitor's Visit method internally uses Accept

            Console.WriteLine("\n--- Traversal Log ---");
            Console.WriteLine(logger.Log);

            Console.WriteLine("\n--- Another Example: Simple arithmetic ---");
            Expression<Func<double , double , double>> calculate = (a , b) => (a * a) + (b / 2);

            var calculatorLogger = new ExpressionLogger();
            calculatorLogger.Visit(calculate);
            Console.WriteLine("\n--- Traversal Log for Calculator ---");
            Console.WriteLine(calculatorLogger.Log);
        }
```

where

`ExpressionLogger` class is defined as follows.

`ExpressionLogger.cs`

```
using System.Linq.Expressions;
using System.Text;

namespace Example.Utilities.Visitor
{
    public class ExpressionLogger : ExpressionVisitor
    {
        private StringBuilder _log = new StringBuilder();
        private int _indentationLevel = 0;

        private void Indent() => _indentationLevel++;
        private void Unindent() => _indentationLevel--;
        private void AppendLine(string message) => _log.AppendLine($"{new string(' ' , _indentationLevel * 2)}{message}");

        public string Log => _log.ToString();

        // Override methods for specific expression types you want to visit
        protected override Expression VisitBinary(BinaryExpression node)
        {
            AppendLine($"Visiting BinaryExpression: NodeType = {node.NodeType}");
            Indent();
            AppendLine($"Left:");
            Visit(node.Left); // Recursively visit the left operand
            AppendLine($"Right:");
            Visit(node.Right); // Recursively visit the right operand
            Unindent();
            return node; // Return the original node or a modified one
        }

        protected override Expression VisitConstant(ConstantExpression node)
        {
            AppendLine($"Visiting ConstantExpression: Value = {node.Value}, Type = {node.Type}");
            return node;
        }

        protected override Expression VisitParameter(ParameterExpression node)
        {
            AppendLine($"Visiting ParameterExpression: Name = {node.Name}, Type = {node.Type}");
            return node;
        }

        protected override Expression VisitLambda<T>(Expression<T> node)
        {
            AppendLine($"Visiting LambdaExpression: Parameters = {string.Join(", " , node.Parameters)}, ReturnType = {node.ReturnType}");
            Indent();
            AppendLine($"Body:");
            Visit(node.Body); // Visit the body of the lambda
            Unindent();
            return node;
        }

        protected override Expression VisitMethodCall(MethodCallExpression node)
        {
            AppendLine($"Visiting MethodCallExpression: Method = {node.Method.Name}");
            Indent();
            if(node.Object != null)
            {
                AppendLine("Object:");
                Visit(node.Object);
            }
            if(node.Arguments.Count > 0)
            {
                AppendLine("Arguments:");
                foreach(var arg in node.Arguments)
                {
                    Visit(arg);
                }
            }
            Unindent();
            return node;
        }
    }
}
```

It will output following

```
In TestMethod49 method call,
Original Expression:
(x, y) => ((((x + y) * 2) > 10) AndAlso (x < 100))

--- Visiting Expression Tree ---

--- Traversal Log ---
Visiting LambdaExpression: Parameters = x, y, ReturnType = System.Boolean
  Body:
  Visiting BinaryExpression: NodeType = AndAlso
    Left:
    Visiting BinaryExpression: NodeType = GreaterThan
      Left:
      Visiting BinaryExpression: NodeType = Multiply
        Left:
        Visiting BinaryExpression: NodeType = Add
          Left:
          Visiting ParameterExpression: Name = x, Type = System.Int32
          Right:
          Visiting ParameterExpression: Name = y, Type = System.Int32
        Right:
        Visiting ConstantExpression: Value = 2, Type = System.Int32
      Right:
      Visiting ConstantExpression: Value = 10, Type = System.Int32
    Right:
    Visiting BinaryExpression: NodeType = LessThan
      Left:
      Visiting ParameterExpression: Name = x, Type = System.Int32
      Right:
      Visiting ConstantExpression: Value = 100, Type = System.Int32


--- Another Example: Simple arithmetic ---

--- Traversal Log for Calculator ---
Visiting LambdaExpression: Parameters = a, b, ReturnType = System.Double
  Body:
  Visiting BinaryExpression: NodeType = Add
    Left:
    Visiting BinaryExpression: NodeType = Multiply
      Left:
      Visiting ParameterExpression: Name = a, Type = System.Double
      Right:
      Visiting ParameterExpression: Name = a, Type = System.Double
    Right:
    Visiting BinaryExpression: NodeType = Divide
      Left:
      Visiting ParameterExpression: Name = b, Type = System.Double
      Right:
      Visiting ConstantExpression: Value = 2, Type = System.Double

```

## reference
### API docs
+ [`Expression.Accept(ExpressionVisitor) Method`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression.accept?view=net-8.0)
+ [`ExpressionVisitor Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expressionvisitor?view=net-8.0)