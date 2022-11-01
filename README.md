# What are Streams?

## Learning Goals

- Understand what streams are and their benefits
- Understand the different types of stream operations

## What is a Stream?

A stream takes a data source (array, collection, file, I/O channel, other streams),
and processes the elements using a sequence of operations combined
into a single pipeline. Streams don’t change the underlying data source;
rather, they provide a result based on the pipelined methods.

Declarative programming is a programming paradigm that abstracts away
explicit control flow (conditionals, loops)
and instead focuses on stating what the task or desired outcome is.
The Stream API was introduced in Java 8.  It allows us to write declarative
and thus more readable code when working with collections and other data sources. 
The Stream API also makes parallelization a lot easier.

We can work with streams in 3 stages:

1. Create a stream from a source.
2. Perform intermediate operations to process the data.
3. Perform a terminal operation to produce a result.

Intermediate operations always return a new stream. We need to perform terminal
operations to get a result or perform a side-effect.

## Stream Creation

There are several ways of creating streams. The common ones are:

1. Creating a stream from a Collection using `stream()`.
2. Creating a stream from arrays using `Arrays.stream()`.
3. Creating streams directly from values using `Stream.of()`.

### Creating a stream from a Collection 

All classes that implement `Collection` have a `stream()` method
for creating streams. This is the most common way to create streams.

```java
import java.util.List;
import java.util.Set;
import java.util.stream.Stream;

public class Example {
    public static void main(String[] args) {
        List<Integer> nums = List.of(1, 2, 3, 4, 5);
        Stream<Integer> numsStream = nums.stream();

        Set<String> languages = Set.of("Python", "Ruby", "C++", "C#", "Go");
        Stream<String> languagesStream = languages.stream();

        System.out.println(numsStream); // java.util.stream.ReferencePipeline$Head@4617c264
        System.out.println(languagesStream); // java.util.stream.ReferencePipeline$Head@36baf30c
    }
}
```

### Creating Streams from Arrays

The `Arrays` class provides a `stream` method for creating streams from arrays.

```java
import java.util.Arrays;
import java.util.stream.IntStream;

public class Example {
    public static void main(String[] args) {
        int[] nums = new int[] {1, 2, 3, 4, 5};
        IntStream numsStream = Arrays.stream(nums);
        System.out.println(numsStream);
    }
}
```

Notice that we’re using `IntStream` instead of `Stream<Integer>`. The `stream`
package also provides `DoubleStream` and `LongStream` classes.

### Stream.of

We can create a stream from values directly using the `Stream.of` method.

```java
import java.util.stream.Stream;

public class Example {
    public static void main(String[] args) {
        Stream<Integer> intStream = Stream.of(1, 2, 3, 4, 5);
        Stream<String> strStream = Stream.of("Rust", "Java", "Go", "Kotlin", "Scala");

        System.out.println(intStream); // java.util.stream.ReferencePipeline$Head@4617c264
        System.out.println(strStream); // java.util.stream.ReferencePipeline$Head@36baf30c
    }
}
```


### `IntStream` vs `Stream<Integer>`

When should we declare a stream to have type `IntStream` versus `Stream<Integer>`?

- A stream created from primitive values (int, double, long) should
  be declared using a primitive stream `IntStream`, `DoubleStream`, and `LongStream`.
- A stream created from objects of type `T` should be declared using an object stream `Stream<T>`.

For example:

```java
    public static void main(String[] args) {

        //int[] --> IntStream
        int[] array1 = {28, 43, 19, 100};
        //IntStream since the array holds primitive int not Integer
        IntStream stream1 = Arrays.stream(array1);

        //Integer[] --> Stream<Integer>
        Integer[] array2 = {28, 43, 19, 100};  //autobox each int to Integer
        //Stream<Integer> since the array holds Integer class instances (autoboxed)
        Stream<Integer> stream2 = Arrays.stream(array2);

        //List<Integer> --> Stream<Integer>
        List<Integer> list1 = List.of(12, 55, 37, 9);
        Stream<Integer> stream3 = list1.stream();

    }
```

## Stream Example

Let’s solve a problem using both loops and streams to see just how much better
streams are when processing data.

### Problem Statement

Given a list of numbers, print the number of even numbers in the list.

```java
public class Example {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    }
}
```

### Explicit Loop and Conditional Solution

Before the introduction of streams, we must provide explicit instruction for every step of the algorithm:

1. Initialize variable `evenCount` to 0.
2. Use a for loop to assign variable `num` to each value in the list.
3. Use a conditional statement to test parity of `num`.
4. Increment `evenCount`.

```java
public class Example {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        long evenCount = 0;
        for (int num: numbers) {
            if (num % 2 == 0) {
                evenCount++;
            }
        }
        System.out.println(evenCount);
    }
}
```

 
### Stream Solution

The stream solution supports a declarative programming style that is much
more readable and easier to read than the loop based solution.

1. Create a stream from the list.
2. Filter the get the even numbers.
3. Count the filtered values.

```java
public class Example {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        long evenCount = numbers.stream()
                .filter(number -> number % 2 == 0)
                .count();
        System.out.println(evenCount);
    }
}
```

We’ll see later that there are several stream operations that make
working with data a breeze!


## Resources

[Java 11 Arrays](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html)  
[Java 11 Collection](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collection.html)  
[Java 11 Stream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/Stream.html)
