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
        /// illustrate how to get events that is declared or inherited by the current Type.
        /// </summary>
        public static void TestMethod33()
        {
            Console.WriteLine("In {0} method call" , MethodBase.GetCurrentMethod().Name);

            int counter = 1;
            Type type;
            BindingFlags bindingFlags;
            BindingFlags[] bindingFlagses;
            EventInfo[] eventInfos;
            string textInfo;

            ///* ------------------ Example 1 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GenericRepository<Person>);

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 2 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(IGenericRepository<Person>);

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 3 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(StatisticsHandler);

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 4 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GreetingEvent);

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 5 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GreetingEvent);

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 6 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GreetingEvent);

            bindingFlags = 
                BindingFlags.NonPublic
                ;
            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.NonPublic
            };

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type, bindingFlagses: bindingFlagses);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;

            ///* ------------------ Example 7 ------------------ *///
            Console.WriteLine(@"///* ------------------ Example {0} ------------------ *///" , counter);

            type = typeof(GreetingEvent);

            bindingFlags =
                BindingFlags.Static
                ;
            bindingFlagses = new BindingFlags [ ]
            {
                BindingFlags.NonPublic
            };

            eventInfos = type.GetEvents();

            textInfo = eventInfos.GetInfo(sourceType: type , bindingFlagses: bindingFlagses);

            Console.WriteLine(textInfo);
            Console.WriteLine();

            counter++;
        }
```

will output following

```
In TestMethod33 method call
///* ------------------ Example 1 ------------------ *///
With these flags:
BindingFlags.Public
There are no such events found in Example.Generics.Repository.GenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type.

///* ------------------ Example 2 ------------------ *///
With these flags:
BindingFlags.Public
There are no such events found in Example.Generics.Repository.IGenericRepository`1[[Example.Bean.Person, Example, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]] type.

///* ------------------ Example 3 ------------------ *///
With these flags:
BindingFlags.Public
There are 1 such events found in Example.Helpers.Numbers.StatisticsHandler type.
0th info:
Name:CalculateEvent
EventHandlerType.FullName:Example.Helpers.Numbers.StatisticsHandler+CalculateEventHandler
DeclaringType.FullName:Example.Helpers.Numbers.StatisticsHandler


///* ------------------ Example 4 ------------------ *///
With these flags:
BindingFlags.Public
There are 1 such events found in Example.Helpers.Events.GreetingEvent type.
0th info:
Name:goodMorningEvent
EventHandlerType.FullName:Example.Helpers.Events.GreetingEvent+GoodMorningEventHandler
DeclaringType.FullName:Example.Helpers.Events.GreetingEvent


///* ------------------ Example 5 ------------------ *///
With these flags:
BindingFlags.Public
There are 1 such events found in Example.Helpers.Events.GreetingEvent type.
0th info:
Name:goodMorningEvent
EventHandlerType.FullName:Example.Helpers.Events.GreetingEvent+GoodMorningEventHandler
DeclaringType.FullName:Example.Helpers.Events.GreetingEvent


///* ------------------ Example 6 ------------------ *///
With these flags:
BindingFlags.NonPublic
There are 1 such events found in Example.Helpers.Events.GreetingEvent type.
0th info:
Name:goodMorningEvent
EventHandlerType.FullName:Example.Helpers.Events.GreetingEvent+GoodMorningEventHandler
DeclaringType.FullName:Example.Helpers.Events.GreetingEvent


///* ------------------ Example 7 ------------------ *///
With these flags:
BindingFlags.NonPublic
There are 1 such events found in Example.Helpers.Events.GreetingEvent type.
0th info:
Name:goodMorningEvent
EventHandlerType.FullName:Example.Helpers.Events.GreetingEvent+GoodMorningEventHandler
DeclaringType.FullName:Example.Helpers.Events.GreetingEvent
```

## reference
### API docs
+ [`Type.GetEvents Method`](https://learn.microsoft.com/en-us/dotnet/api/system.type.getevents?view=net-8.0)

+ [`EventInfo Class`](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.eventinfo?view=net-8.0)