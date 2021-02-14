## Introduction
This is meant as a quick lookup for "How do I do X in Java" for the SystemsDevelopment 2021 course @ Sundhed og Informatik.
Documentation can be hard to read and searching the internet for examples might yield bad or irrelevant examples.
We hope this page will alleviate some frustrations wrt. programming in Java.

We encourage you to give us feedback. If you find errors, think something is missing or "impossible to understand", please let us know. (Or if you are really a pro, fix/add it yourself and send a pull-request on [github](https://github.com/madsobitsoe/Systemudvikling2021-cookbook/tree/gh-pages)!)

## Contents
- [Syntax](#syntax)
- [Java Snippets](#java-snippets)
- [Loops](#loops)
- [ArrayList](#arraylist)
- [Classes](#classes)
- [Objects](#objects)
- [Methods](#methods)
- [The main-method](#the-main-method)



## Syntax
Java is a programming language with a "C-style syntax", meaning the syntax is heavily inspired by the programming language C. Other languages with C-style syntax include C++, C# and javascript.

### Comments
Java has two options for comments.
Single-line comment and multi-line comments.

``` java
// This is a single-line comment - everything to the right of // is ignored

/*
This is a multi-line comment. Everything
between the dash-asterisk and the asterisk-dash is ignored
*/
```

### Curly braces (scope)
Curly braces are used to indicate "scope".
Scope is a complex topic, so think of them as "where things begin and end".
In python, different levels of indentation is used to indicate the same thing.

#### Examples
In an `if-else` construct:
``` java
if (true) {
    //do everything between the braces
}
else {
    // do everything between the braces
}
```

In methods:

``` java
public void someMethod()
{ // this brace starts the "body" of the method



} // This brace ends the "body" of the method
```

In classes:

``` java
public class MyClass
{ // This brace "starts the class"
... // the code making up the body of the class
} // and this brace ends it
```

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


## Loops

### While-loops

A while loop can be used, whenever we want to do 1 thing until a condition is fulfilled.

For example, if we want to keep printing numbers until we reach a certain number:
``` java
int number = 1;
while (number < 10) {
    // Print the current number
    System.out.println("Number: " + number);
    // Increment the number by 1
    number = number + 1;
}
```

Or  if we want to repeatedly ask the user for input, until they provide us with a "valid" input.

``` java
Scanner inputScanner = new Scanner(System.in);
System.out.print("Please enter CPR-number: ");
// Get input from the user
String userInput = inputScanner.nextLine();
// While the input is not valid, keep asking the user for input
while (!isValidCPR(userInput) {
    System.out.print("Not a valid cpr! Please try again: ");
    userInput = inputScanner.nextLine();
}

```

### For-each loops
If we have a collection, e.g. an ArrayList, we can iterate over each item with a for-each loop.
The following code will print out the numbers 1, 2 and 3, on separate lines.

``` java
ArrayList<Integer> numbers = new ArrayList();
numbers.add(1);
numbers.add(2);
numbers.add(3);
for (int number : numbers) {
    System.out.println(number);
}
```

The format of a for-each loop is:

``` java
for ([type] nameInLoop : [theCollection]) {
    // here we can refer to the "current item" by writing nameInLoop
    // The loop body, everything between the curly braces, will be run
    // one time for each of the items, or objects, in theCollection
}

```

### For-loops
If we want to run the same piece of code a certain number of times and we know exactly how many times, we can use a for-loop.

``` java
for (int i = 0; i < 10; i++) {
    System.out.println("Loop iteration: " + i);
}
```

You will typically want to use for-each loops instead.



## ArrayList

An ArrayList is similar to a python-list. We can use them to store objects.

An ArrayList needs to have a type, and all objects in the ArrayList has to be of that type. We cannot mix e.g.  `String` and `Integer`.
An ArrayList can only contain objects, not "primitive data types" such as `int`, `boolean` and `double`. To store numbers, we need to use a "wrapper" class.

ArrayLists are `Collections` - so we can use the methods in `java.util.Collections` to operate on them.

### Creating an ArrayList

To create an ArrayList, we write:

``` java
ArrayList<[type]> nameOfArrayList = new ArrayList();
```

Here, `[type]` is the class/type of object we want to store in the array list. We replace `[type]` with the name of the class.
`nameOfArrayList` is the variable name we want to use for referring to the ArrayList.

If we want a list of integers, we would write:

``` java
ArrayList<Integer> numbers = new ArrayList();
```

or, if we want a list of `String`s to store names:

``` java
ArrayList<String> names = new ArrayList();
```

or, if we want a list of Patient-objects and have a class called `Patient`:
``` java
ArrayList<Patient> patients = new ArrayList();
```


### Examples of use
#### Creating an ArrayList of Integers
This will create an ArrayList, containing `{8, 42, 1990}`

``` java
ArrayList<Integer> numbers = new ArrayList();
numbers.add(8);
numbers.add(42);
numbers.add(1990);
```

#### Reversing an ArrayList
This will reverse the ArrayList, so it will contain `{1990, 42, 8}`

``` java
ArrayList<Integer> numbers = new ArrayList();
numbers.add(8);
numbers.add(42);
numbers.add(1990);
Collections.reverse(numbers);
```

#### Sorting an ArrayList
This will sort the Arraylist `numbers`, so it will contain `{1, 4, 8, 42}`.

``` java
ArrayList<Integer> numbers = new ArrayList();
numbers.add(8);
numbers.add(4);
numbers.add(42);
numbers.add(1);
Collections.sort(numbers);
```


## Structure of a Java program
Java is an Object-Oriented language - thus all programs are structured in terms of classes.
A java program must have a single class containing a `main`-method.



### Classes
A Class can be thought of as a *blueprint* of something we model, describing the data/attributes that make up the object, as well as what methods, or *actions* are part of the object.
Examples are always better.

If we are making a health-care application, we might want to model the concept of a patient and associated data.
A patient would have *data*, e.g. a Name, and Address, a CPR-number etc.

A very simple patient class could look like:

``` java
public class Patient {
	// The data we want to store with each patient, name and age
	private String name;
	private int ageInYears;
	// The constructor - initializes a new object/instance of a patient
	public Patient(String name, int ageInYears) {
		this.name = name;
		this.ageInYears = ageInYears;
	}
	// Methods for accessing the data stored in the patient
	public String getName() {
		return this.name;
	}
	public int getAgeInYears() {
		return this.ageInYears;
	}
}
```

### Objects
When you have a class, you have a *blueprint* of some concept or entity, e.g. the Patient modelled above.

To *use* an object, we need to create it.
We can use the `new` keyword for this.

``` java
// Assuming the Patient class from earlier is available
// Create an instance of the Patient class
Patient myPatient = new Patient("Marvin the Paranoid Android", 42);
// Now we can use and access the object through the name myPatient.
// We will print out the patients name, using the getName()-method
System.out.println("The patients name is: " + myPatient.getName());
// And print out the patients age, using the getAgeInYears-method
System.out.println("The patients age is: " + myPatient.getAgeInYears());
```

### Methods
A method is like a python function. In Java, all functions are tied to classes, i.e. they "exist in a class".

A method follows the format

``` java
[access-modifier] [static*] [return-type] name([arg0type arg0], [arg1type arg1]) {
[method-body]
[return return-value];
}
```
`access-modifier` can be `public`, `private`, `protected`. It defines whether other classes can call the method.
`static` is an optional keyword, used if the method is to be declared static.

`return-type` is the *type* of value this method will return. It can be any of javas simple built-in types, e.g. `int`, `double`, `String`, any class that exists in the codebase, e.g. the Patient class from above or `void`. `void` means the method will not return anything.

`name` is the name of the method - what will we write to call this method.
The parentheses following the `name` contains the arguments to the method.
A method does not need to take any arguments. Arguments are separated by comma.

#### Examples
A method that takes no arguments, and always returns the number 42 - with the type `int`.
``` java
public int get42() {
	return 42;
}
```
A static method that takes one argument: a name with the type `String`.
It uses the name to generate a greeting message, which is returned.
``` java
public static String greet(String name) {
	return ("Hello, " + name + "!");
}
```
A method that takes no arguments and returns a "default patient"
``` java
public Patient getDefaultPatient() {
	Patient p = new Patient("unknown name", 20);
	return p;
}
```

A method in a class Person, that returns the name of the person as a String.
``` java
class Person {

 ... // constructor etc.
	public String getName() {
        return this.name;
	}
}
```

A method in a class Person, that overwrites the name of the person. The new name is supplied as an argument of type `String`.

``` java
class Person {

 ... // constructor etc.
	public void setName(String newName) {
        this.name = newName;
	}
}
```

Most methods (functions) are tied to a specific instance.
The `get`-methods in the Patient-class will return different values for each patient object.



A method can also be `static`. A `static` method is "declared at the class level" as opposed to "declared for each object".

This can be useful in many cases.
Examples:
- We want to create a library of mathematical functions, that can be used without creating an object every time.
- We want to create functions for validating input-data
- We want to obtain a connection to a database

In general, if you want to "do something" that is general and not directly related to a specific object/instance, static is a good choice.



### The main-method
The main method is the point of the program, where the JVM will start execution.

It is a special method that needs to be present. It is not important to understand exactly what it does or what `public static void` means.

The shortest java-program we can write, is this
``` java
public static void main(String[] args) {
	// The code will start executing from here
	return 0;
}
```

`public` is the access-modifier. It means the method (`main`) is accessible from other classes. It needs to be, so the JVM can start the program by calling the main-method.

`static` means the method belongs to the class, as opposed to the object.
It is possible to call the method without instantiating an object. (It is not possible to call the method on an object.

`void` is the return type - or in this case, lack of return type. The method does not "return anything".

`main(String[] args)`

`main` is the name of the method. Inside parentheses are the arguments or parameters for the method.
`String[]` is the `type` of the argument - an array of strings. `args` is *the name* of the first argument. We can use this name to refer to the values inside the main-method.
`args` will contain the command-line arguments that was used to start the program. In most cases, `args` will be empty.
