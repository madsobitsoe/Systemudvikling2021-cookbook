# Week 5 solutions

Programming Exercises

The exercise follows here:

1. A Patient class
Create a class for managing patients ie. the Patient class, with the following private attributes:

- first name
- last name
- CPR number (bonus: how do you ensure that the CPR is written correctly?)
- telephone number (bonus: how do you ensure that this is a valid telephone number?)
- E-mail (bonus: how do you ensure that this is a valid email address?)

Add the following public get- and set-methods: (In IntelliJ IDEA you can use Alt-insert and get the Generate menu that helps you) 

- getFirstname()
- setFirstName(newName)
- getLastName()
- setLastName()
- getCPR()
- setCPR(newCPR)
- getPhoneNumber()
- setPhoneNumber(newPhoneNumber)
- getEmail()
- setEmail(newEmail)
- getAge() - (this attribute is calculated from the CPR number, so there are no setter for this, and no private attribute)

2. Write a main method that exhibits the correct functioning of the patient class (create new patients, retrieve data from the patients, modify attributes of a patient, etc).
3. Create a graphical user interface (GUI) for the patient class in task 1
4. Create an Employee class that represents an employee at the health clinic. An employee has the following attributes 
- First name
- Last name
- Employee id eg mb001 and mb002
- Job description eg doctor, nurse, secretary, physical therapist, chiropractor, smoking cessation consultant or psychologist
- Scheduled days off


**Note that The solution will be presented in the order 1,4,2,3**

## Re 1

The patient class was defined in an earlier week's exercise. We just need to add email and some getters and setters.. (The trivial ones can be created using the Generate (Alt-Insert) menu.

So you will need to create a new JavaFX project (we already covered how to ) and - provided you already have created a patient class - copy the patient class into the new project. 

An example of the **Patient** class can be seen here:

```java
package sample;

import java.time.LocalDate;
import java.time.Period;
import java.time.format.DateTimeFormatter;


public class Patient {

    private String firstName;
    private String lastName;
    private String phoneNumber;
    private String email;
    private String cpr;

    public Patient(String firstName, String lastName, String inputCPR) {
        // assign the inputName to the field storing the patient's name. (replace the ??? with the field your created earlier)
        this.firstName = firstName;
        this.lastName = lastName;

        // assign the inputCPR to the field storing the patient's CPR-number. (replace the ??? with the field your created earlier)
        this.cpr = inputCPR;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getCpr() {
        return cpr;
    }

    public void setCpr(String cpr) {
        this.cpr = cpr;
    }

    public int getAge () {
        DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("ddMMuu");
        LocalDate today = LocalDate.now();
        LocalDate birthdate = LocalDate.parse(cpr.substring(0,6), dateFormatter);
        // If the birthdate is after today, it's probably in the 1900's. Subtract 100 years.
        if (birthdate.isAfter(today)) { birthdate = birthdate.minusYears(100); }
        int age = Period.between(birthdate, today).getYears();
        return age;
    }
    public String getGender() {
        String serial = cpr.substring(7,11);
        if (Integer.parseInt(serial) % 2 == 0) {
            return "female";
        } else {
            return "male";
        }
    }

    @Override
    public String toString() {
        return "Patient{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", phoneNumber='" + phoneNumber + '\'' +
                ", email='" + email + '\'' +
                ", cpr='" + cpr + '\'' +
                '}';
    }
}
``` 

## Re 4

To solve task 4 I have actually created two classes **Holidays** and **Employee** as I could then reuse some of the functionality from the previous week's exercises:

The **Holidays** class can be seen here - explanations are in the code comments.

```java
package sample;

import java.time.LocalDate;
import java.util.ArrayList;

public class Holidays {
    private ArrayList<LocalDate> holidays;

    public Holidays() {
        // The constructor
        this.holidays = new ArrayList<LocalDate>();
    }
    public void addHoliday(LocalDate holiday) {
        // add a holiday to the list of holidays - only if it is not in the list already
        if (! this.holidays.contains(holiday)) {this.holidays.add(holiday);}
    }
    public void removeHoliday(LocalDate holiday) {
        // remove a holiday from the list of holidays - only if it is in the list already
        if (this.holidays.contains(holiday)) {this.holidays.remove(holiday);}
    }

    public ArrayList<LocalDate> getHolidays() { // return a list of holidays
        return holidays;
    }

    public Boolean containsDay(LocalDate holiday) { // Check of the date in in the list of holidays

        return this.holidays.contains(holiday);
    }

    public void printHolidays() {
        System.out.println("Number of holidays: " + this.holidays.size());
            for (LocalDate d : this.holidays) {
                System.out.println(d);
        }

    }

    public Integer getSize() { // Return the number of holidays in the holidays list
        return this.holidays.size();
    }
}
```

Now we are ready to copy the employee class into the project:


```java
package sample;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;


public class Employee {
    private String firstName;
    private String lastName;
    private String employeeId;
    private Holidays holidays;

    public Holidays getHolidays() {
        return holidays;
    }

    public void setHolidays(Holidays holidays) {
        this.holidays = holidays;
    }



    public Employee(String firstName, String lastName, String employeeId) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.employeeId = employeeId;

    }

    public String checkAvailable(Clinic clinic, LocalDate bookingDate){
        String bookingDateAsString=bookingDate.format(DateTimeFormatter.ofPattern("dd/MM/yyyy"));
        if (clinic.getHolidays().containsDay(bookingDate)) {
            return ("The clinic is closed on " +  bookingDateAsString);
        } else if (!(this.holidays==null) && this.holidays.containsDay(bookingDate)) {
            return("Employee is on holiday on " + bookingDateAsString);
        } else {
            return("Employee is available on " + bookingDateAsString);
        }
    };

    public void printHolidays() {
        if (this.holidays != null) {
            System.out.println("Number of holidays: " + this.holidays.getSize());
            if (this.holidays.getSize() > 0) {
                for (LocalDate d : this.holidays.getHolidays()) {
                    System.out.println(d);
                }
            }
        }
    }

}
```

The **Employee** class also contains references to the **Clinic** class (we used it - in a quick and dirty solution - in order to check available holidays for an employee in a clinic) - and why not keep it here, so let us also add the **Clinic** class. (Alternatively you must remove all references for the code to work):

```java
package sample;

public class Clinic {
    // Class to hold a clinic and the associated holidays

    private Integer clinicNumber;
    private Holidays holidays;
    public Clinic(Integer clinicNumber, Holidays inputHolidays) {
        this.clinicNumber = clinicNumber;
        this.holidays = inputHolidays;
    }

    public Holidays getHolidays() {
        return holidays;
    }

    public void setHolidays(Holidays holidays) {
        this.holidays = holidays;
    }
}
```

## Re 2

To solve this task I will introduce a **PatientRegister** class which can generate a list of patients, add and remove patients to the list and has a method to return all patients as a formatted list (and also a method to print the list to standard out.







![](Week5.png)
