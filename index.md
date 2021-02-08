## Introduction

## Java snippets


### Printing to the console
You can use `System.out.print` and `System.out.println` to print to the console. `println` will add a newline at the end.
```java
// This will print "Hello java!" to the console (without the double quotes), and add a newline.
System.out.println("Hello java!");
// This will also print "Hello java!" to the console (without the double quotes), and add a newline.
// The first line prints "Hello"
System.out.print("Hello");
// The second line prints " java!" and adds a newline
System.out.println(" java!");
```

### Reading input from the console
You can use the Scanner class to read input from the user, typed at the console.
You will need to import the class before you can use it
```java
// At the top of the .java file, import the Scanner class
import java.util.Scanner;
...

// Create an instance of the Scanner class
Scanner inputScanner = new Scanner(System.in);
// Read a string from the user
String someString = inputScanner.nextLine();
// Read an int from the user
int someNumber = inputScanner.nextInt();
// Read a double from the user
double someDecimalNumber = inputScanner.nextDouble()
```

### Working with integers
#### Is a number even or odd?
Using the `modulo` operator, we can compute whether a number is even or odd

``` java
int number1 = 42;
int number2 = 17;
// This will be true
boolean number1IsEven = number1 % 2 == 0;
// This will be false
boolean number2IsEven = number2 % 2 == 0;
```

### Working with strings
#### Grabbing a part of a string
You can use the substring-method of the string class to grab part of a string.
You can either supply a `start-index` or both a `start-index` and an `end-index`.
`end-index` is *not* inclusive.
``` java
String letters = "abcdefgh";
// Grab the first 3 letters
// The string starts at index 0 and we want the 3 first letters
String abc = letters.substring(0,3);
// We want to start at index 3 (the 'd' character) and end when we reach index 6 (the 'g' character)
// This will assign "def" to the variable def
String def = letters.substring(3,6);
// We can also just skip the first N letters
// This will assign "defg" to the variable skipABC
String skipABC = letters.substring(3);
```

#### Remove leading and trailing whitespace
You can remove spaces at the beginning and end of a string with the `trim()` method of the String-class.
``` java
String someStringWithSpaces = "   hey   ";
// Remove the leading and trailing spaces from the string
// and assign it to the variable stringWithoutSpaces
String stringWithoutSpaces = someStringWithSpaces.trim();
// stringWithoutSpaces now contains the string "hey"
```



### Working with dates

In java, you can use the `LocalDate` and `LocalDateTime` classes to work with dates.
To do computations, e.g. "how many days/years/months between these two dates?", you can use the `Period` class.
To format a date for printing, or for parsing a date from an input-string, you can use the `DateTimeFormatter` class.

You will need to import the respective classes at the top of your java file.

``` java
// To use the LocalDate class
import java.time.LocalDate;
// To use the Period class
import java.time.Period;
// To use the DateTimeFormatter class
import java.time.format.DateTimeFormatter;
```

#### Getting todays date
``` java
LocalDate today = LocalDate.now();
```
#### Parsing a date from a cpr-string
``` java
// cpr is the string we will be parsing
String cpr = "010295-1234"
// Create a formatter object, that fits the ddmmyy-XXXX pattern of CPR
DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("ddMMuu");
// Parse the first 6 digits in the string assigned to the variable cpr
LocalDate birthdate = LocalDate.parse(cpr.substring(0,6), dateFormatter);
```


#### Calculating number of days, months and years between two dates
If you have two `LocalDate`-objects, you can calculate the period between them with the `Period` class.

``` java
// Create a DatetimeFormatter object
DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("dd-MM-yyyy");
// The 1st of january 2020
LocalDate d1 = LocalDate.parse("01-01-2020", dateFormatter);
// Todays date
LocalDate d2 = LocalDate.today();
int daysSince = Period.between(d1, d2).getDays();
int monthsSince = Period.between(d1, d2).getMonths();
int yearsSince = Period.between(d1, d2).getYears();
```


## Structure of a Java program
Java is an Object-Oriented language - thus all programs are structured in terms of classes.
A java program must have a single class containing a `main`-method. 

### The main-method
The main method is the point of the program, where the JVM will start execution. 

``` java
public static void main(String[] args) {
	return 0;
}
```
