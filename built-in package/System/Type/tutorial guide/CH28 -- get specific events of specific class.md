# CH28 -- get specific events of specific class
## objectives
You will learn how to

+ get specific events of specific class

## CH28.1 -- get specific events of specific class
### `GetEvents` instance method
`GetEvents` instance method will find all events (event type that is defined with `event`) that is declared or inherited by current `Type`.

+ If the flags (`BindingFlags` enum type) is passed as last argument,

it will also filter the event by the flags (see [overloads](https://learn.microsoft.com/en-us/dotnet/api/system.type.getevents?view=net-8.0#system-type-getevents(system-reflection-bindingflags)))

+ Otherwise, it will filter public event (with `public` modifier) (see [overloads](https://learn.microsoft.com/en-us/dotnet/api/system.type.getevents?view=net-8.0#system-type-getevents))

#### Overloads

+ 

```
public virtual System.Reflection.EventInfo[] GetEvents();
```

It will find all events that is declared or inherited by current `Type` and filter public event (with `public` modifier) (see [overloads](https://learn.microsoft.com/en-us/dotnet/api/system.type.getevents?view=net-8.0#system-type-getevents))

+

```
public abstract System.Reflection.EventInfo[] GetEvents(
    System.Reflection.BindingFlags bindingAttr
);
```

It will by name that is declared or inherited by current `Type` and filter the event by the flags (see [overloads](https://learn.microsoft.com/en-us/dotnet/api/system.type.getevents?view=net-8.0#system-type-getevents(system-reflection-bindingflags)))

#### Returned value
It will return `EventInfo[]`.

+ When it finds such event by name of current `Type`, 

it will return a non-empty array of `EventInfo`

+ Otherwise, it will return an non-empty array of `EventInfo`

## examples
### example 1
#### utility class
see demo project for complete code.
#### main code
Invoking following method

```
        /// <summary>
        /// illustrate how to get specific event that is declared or inherited by the current Type.
        /// </summary>
        public static void TestMethod31()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type type;
            string eventNameToFind = string.Empty;
            EventInfo eventInfo;
            string text;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GenericRepository<Person>);

            eventNameToFind = "DeleteOne";
            eventInfo = type.GetEvents(eventNameToFind);

            text = $"There is {eventInfo?.Name ?? "no such"} event in {type.FullName} type";

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(IGenericRepository<Person>);

            eventNameToFind = "DeleteOne";
            eventInfo = type.GetEvents(eventNameToFind);

            text = $"There is {eventInfo?.Name ?? "no such"} event in {type.FullName} type";

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 3 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(StatisticsHandler);

            eventNameToFind = "CalculateEvent";
            eventInfo = type.GetEvents(eventNameToFind);

            text = $"There is {eventInfo?.Name ?? "no such"} event in {type.FullName} type";

            Console.WriteLine(text);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod31 method call
///* ------------------ Example 1 ------------------ *///
There is no such event in Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type

///* ------------------ Example 2 ------------------ *///
There is no such event in Example.Generics.Repository.IGenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type

///* ------------------ Example 3 ------------------ *///
There is CalculateEvent event in Example.Helpers.Numbers.StatisticsHandler type

```

## reference
### API docs
+ [`Type.GetEvents Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getevent?view=net-8.0)

+ [`EventInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.eventinfo?view=net-8.0)

### examples
The code of example 1 are referenced then modified by [the example provided b資Google Gemini](https://g.co/gemini/share/040c487610c1)