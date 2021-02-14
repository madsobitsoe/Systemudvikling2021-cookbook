# Exercises week 2

This week we will repeat the exercise from last week, but with a focus on how to test our code.

The exercises will probably take longer than one hour to complete.
Expected in exercises-session: Task 1, making a unit test and running it.
Expected as "homework": The rest of the exercises.

## Task 1
### Repeat part of the exercise from last week, using a test-driven approach.
We will focus only on validating CPR-numbers in this exercise. "valid" names and addresses are tricky to define precisely, but cpr is straight-forward; at least compared to names and addresses.

We will create a class, DataValidator, that we can use to validate different things.
In this exercise, we will be doing Test-Driven Development (TDD).
That means we will first write tests for the `isValidCPR`-method, then implement enough of that method to make the tests pass - and then repeat this process, until we can validate CPR-numbers.

An example use of the `DataValidator` class:

``` java
public static void main(String[] args) {
 Scanner inputScanner = new Scanner(System.in);
        // Create a DataValidator object
        DataValidator dv = new DataValidator();

        System.out.print("Please enter CPR: ");

        // Get (first attempt) user input (and remove leading and trailing whitespace)
        String input = inputScanner.nextLine().trim();

        // While the input is not valid, repeat
        while (!dv.isValidCPR(input)) {
            System.out.print("Invalid cpr. Try again: ");
            input = inputScanner.nextLine().trim();
        }
        // At this point, the cpr stored in `input` is a valid CPR.
        System.out.println("The entered CPR: " + input);
}
```

#### Creating the DataValidator class.

Create a new class `DataValidator` with three public methods: isValidCPR, isValidName and isValidAddress.
Initially, "stub" the method by making it return false in all cases.
You can use the following template. (Type it yourself - it's good practice!)

``` java
public DataValidator {
    public boolean isValidCPR(String cpr) {
        return false;
    }
    public boolean isValidName(String name) {
        return false;
    }
    public boolean isValidAddress(String address) {
        return false;
    }
}
```


#### Setting up testing

-  Create a new directory in your project called `test`.
Mark the directory as "Test Sources Root"
- Create a Test-class for the DataValidator class (and install JUnit 5.4 if you haven't already)
- Write three individual testcases for the `isValidCPR` method:
    - Give an invalid cpr-number, containing letters. Assert the result is false. (hint: "12631abc" is an invalid format)
    - Give a cpr-number with an invalid format, containing too few characters. Assert the result is false. (hint: "1234567890" is an invalid format, it does not have the hyphen)
    - Give a cpr-number with a valid format. Assert the result is true. (For our purposes, "ddmmyy-XXXX" is a valid format)
- Run the tests. Two of them should "pass", as we expect false. Remember we haven't written the code for the method yet!
- Begin implementing the method. Start by ensuring the length of the string is exactly 11 characters long. If not, the cpr is definitely invalid.
Once all three tests pass, add more testcases. Consider what invalid input could be, write a test-case based on that and ensure your method handles it correctly.

#### Bonus task (a bit harder)
Use the LocalDate-library to parse the given date and ensure the first six numbers make up an actual date. (i.e. 010165-XXXX is valid, 476501-XXXX is not)
To do this, you will need to use exception handling as well, specifically a try-catch block.

You can read about exceptions here: https://www.javatpoint.com/try-catch-block

You can use the following template as a starting point:

``` java
public boolean isValidCPR() {
... // the code you already have is here
    try {
        // Attempt to parse a date with LocalDate here
    }
    catch (java.time.format.DateTimeParseException ex) {
        // If the parsing failed, a DateTimeParseException will be "thrown"
        // And the java program will jump to this piece of code
        // This would mean the date is not a valid date, thus not a valid cpr-number.
    }
}
```
#### Extra-bonus task

Implement the two methods `isValidName` and `isValidAddress`, using test-driven development.
This will require you to define what a valid name and address is.


## Task 2
### ArrayLists

In this task, you will work with ArrayLists.
ArrayLists are similar to lists in python - we can store stuff in them and use them as "containers".

Start by creating a new Java Project.
You can do all of the ArrayList-exercises in the main-method of this new project.

### Task 2.1

- Create an ArrayList
- add the integers (`int`) 1, 1, 2, 5, 8, 13, 21, 34, 55 to the list
- loop over the ArrayList and print out each number on a separate line

### Task 2.2
- Create an ArrayList
- add the integers (`int`) 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 to the list
- create a variable, `int sum = 0;`
- loop over the ArrayList and add each number to the sum.
- Print out the result. (It should be 55)

### Task 2.3

- Create an ArrayList
- add the Strings: `"Hello"`, `"Systemudvikling!"`, `"Look"`, `"at"`, `"me"`, `","`, `"I'm"`, `"using"`, `"ArrayLists"`, `"like"`, `"a"`, `"pro!"`
- loop over the ArrayList and print out each string on the same line. Separate the strings with a space.


### Task 2.4
- Create an ArrayList
- add the Strings: `"pro!"` , `"REAL"` , `"a"` , `"like"` , `"ArrayLists"` , `"using"` , `"I'm"` , `","` , `"me"` , `"at"` , `"Look"` , `"Systemudvikling!"` , `"Hello"`
- reverse the ArrayList, using `Collections.reverse(theListYouWantToReverse)`. (hint: `import java.util.Collections;`)
- loop over the reversed ArrayList and print out each string on the same line. Separate the strings with a space.
Try to get the result to look exactly the same as in the previous exercise.


## Task 3
### Classes, Patients and PatientRegisters

Going back to the project from Task 1, we want to add more classes than just the `DataValidator`.
If we have a valid CPR-number, we would like to create a patient with that CPR-number and store that patient in a patient register.


#### Task 3.1
Create a new class, Patient.
The Patient-class should have fields/attributes for name and cpr.

``` java
public class Patient {
    // Add a field/attribute for the patients name here.
    // The field should be public.
    // The type of the field should be String
    ...
    // Add a field/attribute for the patients CPR-number here.
    // The field should be public.
    // The type of the field should be String
    ...

    // This is the constructor
    //
    public Patient(String inputName, String inputCPR) {
        // assign the inputName to the field storing the patient's name. (replace the ??? with the field your created earlier)
        this.??? = inputName;
        // assign the inputCPR to the field storing the patient's CPR-number. (replace the ??? with the field your created earlier)
        this.??? = inputCPR;
    }
}
```

#### Task 3.2
Create a new class, PatientRegister.
This class will act as a "container" for registered patients.
The PatientRegister should have an ArrayList of Patient-objects, called `patients` and methods to add patients, remove patients and print them.
Use the following template:

``` java
import java.util.ArrayList;

public class PatientRegister {
    // An arraylist that will hold the registered patients
    public ArrayList<Patient> patients;
    // Constructor
    public PatientRegister() {
        this.patients = new ArrayList<Patient>();
    }
    // The method should check if the patient already exists in the patients-list. If not, the patient should be added to the list.
    public void addPatient(Patient patient) {
        // fill in code here
    }
    // The method should remove the patientToRemove from the patients list.
    public void removePatient(Patient patient) {
        // fill in code here
    }
    // The method should first print the current number of patients in the patients-list.
    // Then loop through the patients in the patients-list,
    // and print out their name and cpr.
    public void printPatients() {
        // Fill in code here
    }
}
```

#### Task 3.3
In the main method of your project:

- create
    - A PatientRegister object
    - 3 (or more) Patient objects
- Add the patients to the PatientRegister, using the addPatient-method.
- Call the printPatients-method.
- Remove one (or more) of the patients, using the removePatients method.
- Call printPatients again to print out the remaining patients.

#### Task 3.4 - more methods
- In the Patient-class, add a getAge()-method.
`public int getAge() { ... }`

The method should calculate the the patient's age based on the patients CPR-number.

- In the Patient-class, add a getGender()-method.
`public String getGender() { ... }`

The method should determine the the patient's gender based on the patients CPR-number.
It should return either the String "male" or the String "female".

#### Taks 3.5 - more testing

- Write unit-tests for the getAge-method
- Write unit-tests for the getGender-method
