# CH2 -- specify the visit behavior in `ExpressionVisitor`
## objectives
You will learn how to

+ specify the visit behavior in `ExpressionVisitor`

## CH2.1 -- specify the visit behavior in `ExpressionVisitor`
Step 1:

Since `System.Linq.Expressions.ExpressionVisitor` is an abstract class,

you have to defined a class that inherits `ExpressionVisitor` and implements its all abstract methods.

Its all abstract method:

  +  `VisitParameter` (declaration: `protected override Expression VisitParameter(ParameterExpression node);`)

The basic structure looks like this:

```
    public class ParameterReplacer : ExpressionVisitor
    {
        protected override Expression VisitParameter(ParameterExpression node)
        {
            // ... logic
            // return an expression.
        }
    }
```

Step 2:

Then put your logic in `VisitParameter` method to specify the behavior of visiting a parameter expression in an expression true.

## examples
### example 1
#### full code about helper class
`ParameterReplacer.cs`

```
using System.Linq.Expressions;

namespace Example.Utilities.Visitor
{
    // Helper class to replace parameters in an expression tree
    public class ParameterReplacer : ExpressionVisitor
    {
        private readonly ParameterExpression _oldParameter;
        private readonly ParameterExpression _newParameter;

        public ParameterReplacer(
            ParameterExpression oldParameter , 
            ParameterExpression newParameter
        )
        {
            this._oldParameter = oldParameter;
            this._newParameter = newParameter;
        }

        protected override Expression VisitParameter(ParameterExpression node)
        {
            return node == _oldParameter ? _newParameter : base.VisitParameter(node);
        }
    }
}
```

#### main method
Invoking following method

```
        /// <summary>
        /// illustrate how to create an expression about a condition.
        /// </summary>
        public static void TestMethod14()
        {
            Console.WriteLine("In {0} method call," , MethodBase.GetCurrentMethod().Name);

            string [ ] textArray = new string [ ]
            {
                string.Empty,
                "Cat",
                "CatShabo",
                "Spy x family",
                "PurismMagica"
            };

            ParameterExpression parameterExpression = Expression.Parameter(typeof(string) , "arg");

            Expression<Func<string , string>> capatalize = text => text.ToUpper();
            Expression<Func<string , string>> uncapatalize = text => text.ToLower();

            Expression<Func<string , bool>> startWithCatLambda = text => string.IsNullOrEmpty(text)== false && text.StartsWith("Cat") ;

            List<ConditionalExpression> conditionalExpressions = new List<ConditionalExpression>();

            // To use startWithCatLambda as the condition, we need to apply it to our 'arg' parameter.
            // We do this by invoking its body with the 'arg' parameter.

            // 1. Get the body of the startWithCatLambda
            Expression conditionBody = startWithCatLambda.Body;

            // 2. Identify the parameter of the startWithCatLambda (the 'text' in text => ...)
            ParameterExpression startWithCatParameter = startWithCatLambda.Parameters [0];

            // 3. Create a parameter replacer visitor to replace 'text' with 'arg' in the condition body
            ParameterReplacer visitor = new ParameterReplacer(startWithCatParameter, parameterExpression);
            Expression actualCondition = visitor.Visit(conditionBody);

            // --- "True" Part (IfTrue) ---
            // 1. Get the body of the capatalize lambda
            Expression capatalizeBody = capatalize.Body;

            // 2. Identify the parameter of the capatalize lambda
            ParameterExpression capatalizeParameter = capatalize.Parameters [ 0 ];

            // 3. Create a parameter replacer for the capatalize body
            ParameterReplacer capatalizeVisitor = new ParameterReplacer(capatalizeParameter , parameterExpression);
            Expression actualCapatalizeBody = capatalizeVisitor.Visit(capatalizeBody);

            // --- "False" Part (IfFalse) ---
            // 1. Get the body of the uncapatalize lambda
            Expression uncapatalizeBody = uncapatalize.Body;

            // 2. Identify the parameter of the uncapatalize lambda
            ParameterExpression uncapatalizeParameter = uncapatalize.Parameters [ 0 ];

            // 3. Create a parameter replacer for the uncapatalize body
            ParameterReplacer uncapatalizeVisitor = new ParameterReplacer(uncapatalizeParameter , parameterExpression);
            Expression actualUncapatalizeBody = uncapatalizeVisitor.Visit(uncapatalizeBody);

            conditionalExpressions.Add(
                Expression.Condition(
                    actualCondition ,
                    actualCapatalizeBody ,  // Use the transformed body
                    actualUncapatalizeBody // Use the transformed body
                )
           );

            
            conditionalExpressions.ForEach(
                (conditionalExpression) =>
                {
                    LambdaExpression lambdaExpression = Expression.Lambda(
                        conditionalExpression,
                        new List<ParameterExpression>() { parameterExpression }
                    );
                    var delegateFunction = lambdaExpression.Compile();

                    foreach(string text in textArray)
                    {
                        Console.WriteLine("delegateFunction.DynamicInvoke({0}):{1}" , text , delegateFunction.DynamicInvoke(text));
                    }
                }
            );
        }
```

It will output following

```
In TestMethod14 method call,
delegateFunction.DynamicInvoke():
delegateFunction.DynamicInvoke(Cat):CAT
delegateFunction.DynamicInvoke(CatShabo):CATSHABO
delegateFunction.DynamicInvoke(Spy x family):spy x family
delegateFunction.DynamicInvoke(PurismMagica):purismmagica
```

## reference
### API docs
+ [`ExpressionVisitor Class`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.expressions.expressionvisitor?view=net-8.0)
