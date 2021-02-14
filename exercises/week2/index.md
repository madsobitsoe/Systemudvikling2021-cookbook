# Exercises week 2

This week we will repeat the exercise from last week, but with a focus on how to test our code.
FILL OUT
Expected in exercises: Task 1, making a unit test and running it.
Expected as "homework": The rest.

## Task 1
### Repeat part of the exercise from last week, using a test-driven approach.
We will focus only on validating CPR-numbers in this exercise. "valid" names and addresses are tricky to define precisely, but cpr is straight-forward; at least compared to names and addresses.

We will create a class, DataValidator, that we can use to validate different things.
An example use:

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

1.1.1) Create a new class `DataValidator` with three public methods: isValidCPR, isValidName and isValidAddress.
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



1.1.2) Create a new directory in your project called "test".
Mark the directory as "Test Sources Root"
1.1.3) Create a Test-class for the DataValidator class (and install JUnit 5.4 if you haven't already)
1.1.4) Write three individual testcases for the `isValidCPR` method:
1.1.4.1) Give an invalid cpr-number, containing letters. Assert the result is false. (hint: "12631abc" is an invalid format)
1.1.4.2) Give a cpr-number with an invalid format, containing too few characters. Assert the result is false. (hint: "1234567890" is an invalid format, it does not have the hyphen)
1.1.4.3) Give a cpr-number with a valid format. Assert the result is true. (For our purposes, "ddmmyy-XXXX" is a valid format)
1.1.5) Run the tests. Two of them should "pass", as we expect false. Remember we haven't written the code for the method yet!
1.1.6) Begin implementing the method. Start by ensuring the length of the string is exactly 11 characters long. If not, the cpr is definitely invalid.
Once all three tests pass, add more testcases. Consider what invalid input could be, write a test-case based on that and ensure your method handles it correctly.
1.1.7) Use the LocalDate-library to parse the given date and ensure the first six numbers make up an actual date. (i.e. 010165-XXXX is valid, 476501-XXXX is not)


## Task 2
### ArrayLists

Look at the examples with arrayList (TODO - examples!).
Type them in and run them.

1.2.1)
- Create an ArrayList
- add the integers (`int`) 1, 1, 2, 5, 8, 13, 21, 34, 55 to the list
- loop over the ArrayList and print out each number on a separate line
1.2.2)
- Create an ArrayList
- add the integers (`int`) 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 to the list
- create a variable, `int sum = 0;`
- loop over the ArrayList and add each number to the sum.
- Print out the result. (It should be 55)

1.2.3)
- Create an ArrayList
- add the Strings: "Hello", "Systemudvikling!", "Look", "at", "me", "," "I'm", "using", "ArrayLists", "like", "a", "pro"
- loop over the ArrayList and print out each string on the same line. Separate the strings with a space.


1.2.4)
- Create an ArrayList
- add the Strings: "pro" , "REAL" , "a" , "like" , "ArrayLists" , "using" , "I'm" , "," , "me" , "at" , "Look" , "Systemudvikling!" , "Hello"
- reverse the ArrayList (using the .reverse() method)
- loop over the reversed ArrayList and print out each string on the same line. Separate the strings with a space.
Try to get the result to look exactly the same as in the previous exercise.


## Task 3
### Classes, Patients and PatientRegisters


1.3.1) Create a new class, Patient.
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

1.3.2) Create a new class, PatientRegister.
This class will act as a "container" for registered patients.
The PatientRegister should have an ArrayList of Patient-objects, called `patients` and methods to add patients, remove patients and print them.
Use the following template:

``` java
import java.util.ArrayList;

public class PatientRegister {
    // An arraylist that will hold the registered patients
    public ArrayList<Patient> patients;
    // Constructor - EXPLAIN
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

1.3.3) In the main method,
1.3.3.1)
create
- A PatientRegister object
- 3 (or more) Patient objects
1.3.3.2) Add the patients to the PatientRegister, using the addPatient-method.
1.3.3.3) Call printPatients.
1.3.3.4) Remove one (or more) of the patients, using the removePatients method.
1.3.3.5) Call printPatients to print out the remaining patients.


## Task 4
### Adding methods and more testing

1.4.1)
In the Patient-class, add a getAge()-method. (public int getAge() { ... })
The method should calculate the the patient's age based on the patients CPR-number.
1.4.2)
In the Patient-class, add a getGender()-method. (public String getGender() { ... })
The method should determine the the patient's gender based on the patients CPR-number.
It should return either the String "male" or the String "female".
1.4.3) Write unit-tests for the getAge-method
1.4.4) Write unit-tests for the getGender-method
