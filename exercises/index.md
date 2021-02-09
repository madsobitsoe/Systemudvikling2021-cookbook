# Solutions for exercises
- [Week 1](#week-1)


## Week 1
### Task 2
> Make a program that calculates the cost of an admission of X days length to the price Y per day.


#### Solution

### Task 3
> Create a program that tells a patient whether his blood percentage (Links to an external site.) requires attention.  Hints? Java if documentation (Links to an external site.)


#### Solution

### Task 4
> Create a program that registers patient data: Name and CPR number and address, and determines the patient age and his/her gender
> 1. Create a new Java Project in IntelliJ
> 2. Use the “Console Application” template
> 3. In the programs main-method, create variables to store patient data. Name, CPR and Address. Consider what data types to use.
> 4. Using the scanner class, ask the user to input Patient data: Name, CPR and Address.
> 5. Determine the age of the patient
> 6. Determine the assigned gender of the patient
> 7. Print out the information to the console


#### Solution

``` java
import java.time.LocalDate;
import java.time.Period;
import java.time.format.DateTimeFormatter;
import java.util.Scanner;

public class Main {
    // Create a program that registers patient data:
    // Name and CPR number and address,
    // and determines the patient age and his/her gender

    public static void main(String[] args) {
        // Create a scanner that reads from STDIN
        Scanner inputscanner = new Scanner(System.in);
        String name;
        String cpr;
        String address;

        System.out.println("Please enter the patients name, cpr-number and address.");
        System.out.print("Name: ");
        name = inputscanner.nextLine();
        System.out.print("CPR: ");
        cpr = inputscanner.nextLine();
        System.out.print("Address: ");
        address = inputscanner.nextLine();
        System.out.println("The patients name: " + name +", cpr: " + cpr + ", address: " + address);
        // Create a custom datetimeformatter for the cpr string
        DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("ddMMuu");
        LocalDate today = LocalDate.now();
        LocalDate birthdate = LocalDate.parse(cpr.substring(0,6), dateFormatter);
        // If the birthdate is after today, it's probably in the 1900's. Subtract 100 years.
        if (birthdate.isAfter(today)) { birthdate = birthdate.minusYears(100); }
        System.out.println("Today:  "+ today.toString());
        System.out.println("Birthdate:  "+ birthdate.toString());

        int age = Period.between(birthdate, today).getYears();
        boolean assignedGenderIsFemale;
        // Grab the last digit of the cpr-string, convert it to an integer
        int lastDigitInCpr = Integer.parseInt(cpr.substring(9));
        // If the last digit is an even number, cpr-assigned gender is female
        assignedGenderIsFemale = lastDigitInCpr % 2 == 0;
        System.out.println("The patient is " + age + " years old.");
        System.out.print("The patients assigned gender is: ");
        if (assignedGenderIsFemale) {
            System.out.println("female");
        }
        else {
            System.out.println("male");
        }
    }
}
```




### Task 5
> Modify the program you created in task 4 to perform minimal error checking.
>
> Ensure that:
>
> - A name can only consist of Letters, not numbers.
> - A name is at least a first- and last name
> - A CPR follows the format ddmmyy-XXXX
> - If the input patient data does not live up to the requirements, notify the user by printing a message.
>
> - If the data fulfills the requirements, print out the patient data, age and assigned gender as in task 4.
> - Consider using methods to perform your validation.
>
> - *Bonus requirement*: Check that the CPR number is valid with Mod11. (https://da.wikipedia.org/wiki/Modulus_11 (Links to an external site.)) (https://da.wikipedia.org/wiki/CPR-nummer#Kontrolciffer_(det_gamle_CPR-nummer) (Links to an external site.))


#### Solution
