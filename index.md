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

