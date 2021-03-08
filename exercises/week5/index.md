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


## Re 1

The patient class was defined in an earlier week's exercise. We just need to add email and some getters and setters.. (The trivial ones can be created using the Generate (Alt-Insert) menu.

So you will need to create a new JavaFX project (we already covered how to ) and - provided you already have created a patient class - copy the patient class into the new project. 

An example of the Patient Class can be seen here:

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





![](Week5.png)
