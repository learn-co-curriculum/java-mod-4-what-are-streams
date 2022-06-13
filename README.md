# What are Streams?

## Learning Goals

- Understand what streams are and their benefits
- Understand the different types of stream operations

## What is a Stream?

The Stream API was introduced in Java 8. It allows us to write more declarative
code which makes it significantly more readable. Furthermore, it makes
parallelization a lot easier.

A stream takes a source (collection, file, I/O channel, other streams),
processes the elements using a sequence of operations combined into a single
pipeline.

We can work with streams in 3 stages:

1. Create a stream from a source
2. Perform intermediate operations to process the data
3. Perform a terminal operation to produce a result

Intermediate operations always return a new stream. We need to perform terminal
operations to get a result or perform a side-effect.

## Stream Creation

There are several ways of creating streams. Some of the common ones are:

1. Calling the `stream()` method on collections
2. Creating a stream from arrays using `Arrays.stream`
3. Creating streams directly from values

### Creating Streams from Collections

All collections have a `stream()` method for creating streams. This is the most
common way to create streams.

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

### Loop Solution

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

The stream solution is much more readable and easier to read than the loop based
solution. We’ll see later that there are several stream operations that make
working with data a breeze!
