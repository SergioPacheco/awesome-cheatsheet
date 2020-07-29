# Java 8 General Knowledge

Q1. What New Features Were Added in Java 8?

Java 8 shipt several new features but the most significante are the following: 

- **Lambdas Expressions** - a new languague feature allowing treating actions as objects.
- **Methods References** - enable defining Lambda Expressions by referring to methods directy using theier names.
- **Functional Interface** - an interface with maximum one abstract method, implementation can be provides using a Lambda Expression.
- **Default methods** - give us the ability to add full implementation in interfaces besides abstact methods
- **Nashorn, JavaScript Engine** - Java-based engine for executing and evaluating JavaScript code.
- **Stream API** - an improved, immutable JodaTime-inspired Date API

Along whit these new features, lots of feature enchancements are done uder-the-hood, at both compiler and JVM level.

# Method Reference

## Q1. Whast Is a Method Reference?

A method reference is a Java 8 construct that can be used for referring a method without invoking it. It is used for treating methods as Lambda Expressions. They only work as syntactic sugar to reduce the verbosity of some lambdas. This way, the follwing code:

> (o) -> o.toString();

can become:

> Obect::toString();

A method reference can be indentified by a double colon separating a class or object name and the name of the method. It has diferent variations such as constructor reference:

> String::new;

Static method reference:

> String::valueOf;

Bound instance method reference:

> str::toString;

Unbound instance method reference: 

> String::toString;

## Q2 What Is the Meaning of String::ValueOf Expression? 

It is a static method reference to the valueOf method of the String Class.

# Optional 

## Q1. What is Optional? How Can It Be Used? 

Optional is a new class in Java 8 that encapsulates an optional value i.e. a value that is either there or not. It is a wrapper around an object, and you can think of it as a container of zero or one element.

Optional has a special Optional.empty() value instead of wrapped null. Thus it can be used instead of a nullable value to get rid of NullPointerException in many cases.

The main purpose of Optional, as designed by its creators, was to be a return type of methods that previously would return null. Such methods would require you to write boilerplate code to check the return value and sometimes could forget to do a defensive check. In Java 8, an Optional return type explicitly requires you to handle null or non-null wrapped values differently.

For instance, the Stream.min() method calculates the minimum value in a stream of values. But what if the stream is empty? If it was not for Optional, the method would return null or throw an exception.

But it returns an Optional value which may be Optional.empty() (the second case). This allows us to easily handle such case:

```Java
    int min1 = Arrays.stream(new int[]{1, 2, 3, 4, 5})
    .min()
    .orElse(0);
    assertEquals(1, min1);

    int min2 = Arrays.stream(new int[]{})
    .min()
    .orElse(0);
    assertEquals(0, min2);
```

# Functional Interfaces

## Q1. Describe Some of the Functional Interfaces in the Standard Library

There are a lot of functional interfaces in the java.util.function package, the more common ones include but not limited to:

- **Function** – it takes one argument and returns a result
- **Consumer** – it takes one argument and returns no result (represents a side effect)
- **Supplier** – it takes not argument and returns a result
- **Predicate** – it takes one argument and returns a boolean
- **BiFunction** – it takes two arguments and returns a result
- **BinaryOperator** – it is similar to a BiFunction, taking two arguments and returning a result. The two arguments and the result are all of the same types
- **UnaryOperator** – it is similar to a Function, taking a single argument and returning a result of the same type

## Q2. What Is a Functional Interface? What Are the Rules of Defining a Functional Interface?

A functional interface is an interface with no more, no less but one single abstract method (default methods do not count).

Where an instance of such interface is required, a Lambda Expression can be used instead. More formally put: Functional interfaces provide target types for lambda expressions and method references.

The arguments and return type of such expression directly match those of the single abstract method.

For instance, the Runnable interface is a functional interface, so instead of:

```Java
Thread thread = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello World!");
    }
});
```

you could simply do: 

```Java
    Thread thread = new Thread(() -> System.out.println("Hello World!"));
```

# Default Method

## What Is a Default Method and When Do We Use It?

A default method is a method with an implementation – which can be found in an interface.

We can use a default method to add a new functionality to an interface while maintaining backward compatibility with classes that are already implementing the interface:

```Java
    public interface Vehicle {
        public void move();
        default void hoot() {
            System.out.println("peep!");
        }
    }
```

Usually, when a new abstract method is added to an interface, all implementing classes will break until they implement the new abstract method. In Java 8, this problem has been solved by the use of default method.

For example, Collection interface does not have forEach method declaration. Thus, adding such method would simply break the whole collections API.

Java 8 introduces default method so that Collection interface can have a default implementation of forEach method without requiring the classes implementing this interface to implement the same.

## Q2. Will the Follwing Code Compile?

```Java
@FunctionalInterface
public interface Function2<T, U, V> {
    public V apply(T t, U u);

    default void count() {
        // increment counter
    }
}
```
Yes. The code will compile because it follows the functional interface specification of defining only a single abstract method. The second method, count, is a default method that does not increase the abstract method count.

# Lambda Expressions

## Q1. What Is a Lambda Expression and What Is It Used For

In very simple, a lambda expressions is a functions that can be referenced and passed around as an object.

Lambda expressions introduce functional style processing in Java and facilitate the writing of compact and easy-to-read code.

Because of this, lambda expressions are a natural replacement for anonymous classes as method arguments. One of their main uses is to define inline implementations of functional interfaces.

## Q2. Explain the Syntax and Characteristics of a Lambda Expression

A lambda expression consists of two parts: the parameter part and the expressions part separated by a forward arrow as below:

> params -> expressions 

Any lambda expression has the following characteristics:

- **Optional type declaration** – when declaring the parameters on the left-hand side of the lambda, we don't need to declare their types as the compiler can infer them from their values. So int param -> … and param ->… are all valid
- **Optional parentheses** – when only a single parameter is declared, we don't need to place it in parentheses. This means param -> … and (param) -> … are all valid. But when more than one parameter is declared, parentheses are required
- **Optional curly braces** – when the expressions part only has a single statement, there is no need for curly braces. This means that param – > statement and param – > {statement;} are all valid. But curly braces are required when there is more than one statement
- **Optional return statement** – when the expression returns a value and it is wrapped inside curly braces, then we don't need a return statement. That means (a, b) – > {return a+b;} and (a, b) – > {a+b;} are both valid

# Nashorn Javascript

## Q1. What Is Nashorn in Java8?

**Nashorn** is the new Javascript processing engine for the Java platform that shipped with Java 8. Until JDK 7, the Java platform used Mozilla Rhino for the same purpose. as a Javascript processing engine.

Nashorn provides better compliance with the ECMA normalized JavaScript specification and better runtime performance than its predecessor.

## Q2. What Is JJS?

In Java 8, **jjs** is the new executable or command line tool used to execute Javascript code at the console.


# Streams 

## Q1. What Is a Stream? How Does It Differ from a Collection? 

In simple terms, a stream is an iterator whose role is to accept a set of actions to apply on each of the elements it contains.

The stream represents a sequence of objects from a source such as a collection, which supports aggregate operations. They were designed to make collection processing simple and concise. Contrary to the collections, the logic of iteration is implemented inside the stream, so we can use methods like map and flatMap for performing a declarative processing.

Another difference is that the Stream API is fluent and allows pipelining:

```Java
    int sum Arrays.stream(new int[]{1,2,3})
        .filter(i -> i >=2)
        .map(i -> i * 3)
        .sum();
```

And yet another important distinction from collections is that streams are inherently lazily loaded and processed.

## Q2. What Is the Difference Between Intermediate and Terminal Operations?

Stream operations are combined into pipelines to process streams. All operations are either intermediate or terminal.

Intermediate operations are those operations that return **Stream** itself allowing for further operations on a stream.

These operations are always lazy, i.e. they do not process the stream at the call site, an intermediate operation can only process data when there is a terminal operation. Some of the intermediate operations are **filter, map and flatMap**.

Terminal operations terminate the pipeline and initiate stream processing. The stream is passed through all intermediate operations during terminal operation call. Terminal operations include **forEach, reduce, Collect and sum.**

To drive this point home, let us look at an example with side effects:

```Java
    public static void main(String[] args) {
        System.out.println("Stream without terminal operation");
        Arrays.stream(new int[] { 1, 2, 3 }).map(i -> {
            System.out.println("doubling " + i);
            return i * 2;
        });
        System.out.println("Stream with terminal operation");
            Arrays.stream(new int[] { 1, 2, 3 }).map(i -> {
                System.out.println("doubling " + i);
                return i * 2;
        }).sum();
    }
```

The output will be as follows:

> Stream without terminal operation
> Stream with terminal operation
> doubling 1
> doubling 2
> doubling 3

## Q3. What Is the Difference Between Map and flatMap Stream Operation?

There is a difference in signature between **map** and **flatMap**. Generally speaking, a **map* operation wraps its return value inside its ordinal type while flatMap does not.

For example, in **Optional**, a **map** operation would return Optional<String> type while **flatMap** would return String type.

So after mapping, one needs to unwrap (read “flatten”) the object to retrieve the value whereas, after flat mapping, there is no such need as the object is already flattened. The same concept is applied to mapping and flat mapping in **Stream**.

Both map and flatMap are intermediate stream operations that receive a function and apply this function to all elements of a stream.

The difference is that for the map, this function returns a value, but for flatMap, this function returns a stream. The flatMap operation “flattens” the streams into one.

Here's an example where we take a map of users' names and lists of phones and “flatten” it down to a list of phones of all the users using flatMap:

```Java 
    Map<String, List<String>> people = new HashMap<>();
    people.put("John", Arrays.asList("555-1123", "555-3389"));
    people.put("Mary", Arrays.asList("555-2243", "555-5264"));
    people.put("Steve", Arrays.asList("555-6654", "555-3242"));

    List<String> phones = people.values().stream()
    .flatMap(Collection::stream)
        .collect(Collectors.toList());
```

## What Is Stream Pipelining in Java 8?

Stream pipelining is the concept of chaining operations together. This is done by splitting the operations that can happen on a stream into two categories: intermediate operations and terminal operations.

Each intermediate operation returns an instance of Stream itself when it runs, an arbitrary number of intermediate operations can, therefore, be set up to process data forming a processing pipeline.

There must then be a terminal operation which returns a final value and terminates the pipeline.

# Java 8 Date and Time API

## Q1. Tell Us About the New Date and Time API in Java 8

A long-standing problem for Java developers has been the inadequate support for the date and time manipulations required by ordinary developers.

The existing classes such as java.util.Date and SimpleDateFormatter aren’t thread-safe, leading to potential concurrency issues for users.

Poor API design is also a reality in the old Java Data API. Here's just a quick example – years in java.util.Date start at 1900, months start at 1, and days start at 0 which is not very intuitive.

These issues and several others have led to the popularity of third-party date and time libraries, such as Joda-Time.

In order to address these problems and provide better support in JDK, a new date and time API, which is free of these problems, has been designed for Java SE 8 under the package java.time.


