# Imperative vs declarative

In short:

- *imperative* means how do something
- *decarative* what to do

Supose we have to find longest word in text eg

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

Step to do:
- split by space
- count lenght of word
- find max

## Imperative way

```java

public class StringSamples {

  public static void main(String[] args) {

    String text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.";

    int max = getMaxWordLength(text);

    System.out.println(max);
  }

  private static int getMaxWordLength(String text) {
    String[] samples = text.split(" ");
    int max = 0;
    for (String sample : samples) {
      max = Math.max(max, sample.length());
    }
    return max;
  }
}

```

A few days ago I reviewed really close code and it encourage me to write this note. Java 8 provide Stream which is good way to code this in declarative way - not so nice as Kotlin but still better then above example.

## Declarative way

```java

public class StringSamples {

  public static void main(String[] args) {

    String text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.";

    int max = getMaxWordLength(text);

    System.out.println(max);
  }

  private static int getMaxWordLength(String text) {
    return Stream.of(text.split(" "))
        .mapToInt(String::length)
        .max()
        .orElse(0);
  }
}
```

What a huge diffrence and clean function.

## Add more complex rules

Supose we search word starting with 'q'

### Imperative

```java

private static int getMaxWordLength2(String text) {
    String[] samples = text.split(" ");
    int max = 0;
    for (String sample : samples) {
      if(sample.startsWith("q")) {
        max = Math.max(max, sample.length());
      }
    }
    return max;
  }

```

Mess...

### Declarative

```java

private static int getMaxWordLength(String text) {
    return Stream.of(text.split(" "))
        .filter(s -> s.startsWith("q"))
        .mapToInt(String::length)
        .max()
        .orElse(0);
  }

  ```

  Nice and clean just by adding filter function.

# Kotlin rocks

Let do the same in Kotlin

```kotlin

fun main() {

    val text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

    val max = text.split("\\s".toRegex())
            .filter { it.startsWith('q') }
            .map { it.length }
            .max()

    println(max)

}

```