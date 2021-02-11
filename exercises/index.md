# Solutions for exercises
- [Week 1](#week-1)
  * [Task 2](#task-2)
  * [Task 3](#task-3)
  * [Task 4](#task-4)
  * [Task 5](#task-5)


## Week 1
### Task 2
> Make a program that calculates the cost of an admission of X days length to the price Y per day.


#### Solution

``` java
import java.util.Scanner;

public class Main {
    // Exercise 2: Make a program that calculates the
    // cost of an admission of X days length to the price Y per day
    // Extend the exercise to take inputs as decimal numbers and not only integers45!

    public static void main(String[] args) {
        // Create a scanner that reads from STDIN
        Scanner inputscanner = new Scanner(System.in);
        int days ;
        int cost;
        int totalPrice;
        System.out.println("Please enter the number of days, and cost pr day");
        System.out.print("Days: ");
        days = inputscanner.nextInt();
        System.out.print("Cost pr day: ");
        cost = inputscanner.nextInt();
        totalPrice = days * cost;
        System.out.println("The total price is: " + totalPrice);
    }
}
```


### Task 3
> Create a program that tells a patient whether his blood percentage (Links to an external site.) requires attention.  Hints? Java if documentation (Links to an external site.)


#### Solution

``` java
import java.util.Scanner;

public class Main {
    // Exercise 3:
    // Create a program that tells a patient whether his/her
    // blood percentage (Links to an external site.) requires attention.
    // Man måler hæmoglobinniveauet ved at tage en blodprøve.
    // Normalområdet for koncentrationen af hæmoglobin hos voksne:
    //
    // Male: 8.3-10.5 mmol/L
    // Female: 7.3-9.5 mmol/L
    //
    // Notice that decimal(floating) numbers must be entered as "," (e.g 9,5)
    // in the console if using danish locale
    //
    // E.g. try to enhance the code to ignore the case of the input!
    // or to keep asking in a loop until the user types a special character

    public static void main(String[] args) {
	    // Create a scanner that reads from STDIN
        Scanner inputscanner = new Scanner(System.in);
        Character c =' ' ;
        float bp;
        int totalPrice;

        double bpMaleLowWaterMark = 8.3;
        double bpMaleHighWaterMark = 10.5;

        double bpFemaleLowWaterMark = 7.3;
        double bpFemaleHighWaterMark = 9.5;

        System.out.println("Please enter your sex (M/F), and blod percentage in mmol/L");
        System.out.print("Sex (M/F):");
        c = inputscanner.next().charAt(0);

        System.out.print("Blod percentage in mmol/L: ");
        bp = inputscanner.nextFloat();

        if (c.equals('M')) {
            if (bp <= bpMaleLowWaterMark) {
                System.out.println("Your blood percentage is too low\nPlease see a doctor");
            }
            else if (bpMaleLowWaterMark <= bp &&  bp <= bpMaleHighWaterMark) {
                System.out.println("Your blood percentage is fine");
            } else {
                System.out.println("Your blood percentage is too high\nPlease see a doctor");
            }
        } else if (c.equals('F')) {
            if (bp <= bpFemaleLowWaterMark) {
                System.out.println("Your blood percentage is too low\nPlease see a doctor");
            }
            else if (bpFemaleLowWaterMark <= bp && bp <= bpFemaleHighWaterMark) {
                System.out.println("Your blood percentage is fine");
            } else {
                System.out.println("Your blood percentage is too high\nPlease see a doctor");
            }
        } else {
            System.out.println("Sex must be M (Male) or F (Female)");
        }
    }
}
```


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

**note:** This is *one way* to do this. The exercise is very loose wrt. requirements and the learning goals are:
1. Get acquainted with Java
2. Realise that parsing data from a user is a massive endeavour

If any of the code is confusing, please ask Mads to explain it in the exercise classes.

``` java
package com.company;

import java.time.LocalDate;
import java.time.Period;
import java.time.format.DateTimeFormatter;
import java.util.Arrays;
import java.util.Scanner;
import java.util.stream.Collectors;


public class Main {
    /*
    Modify the program you created in task 4 to perform minimal error checking.
    Ensure that:
    - A name can only consist of Letters, not numbers.
    - A name is at least a first- and last name
    - A CPR follows the format ddmmyy-XXXX
    - If the input patient data does not live up to the requirements, notify the user by printing a message.

    - If the data fulfills the requirements, print out the patient data, age and assigned gender as in task 4.
    Consider using methods to perform your validation.
     */
    static Scanner inputscanner;

    // A static method to get a patient's name
    // This can get quite complicated, as we should handle
    // øæå (and ÿ and ö and every other special letter)
    // and potential middle names
    public static String getName() {
        int max_length = 80;
        boolean valid = false;
        String input = "";
        // A string-array to hold all the names of the input
        // Initialize it to an empty array
        String[] names = {};
        System.out.print("Please enter the patients name: ");
        while (!valid) {
            input = inputscanner.nextLine();
            // We assume all names are at least two characters long
            // and a patient must have at least two names, separated by at least one space
            // so the input must be at least 5 characters long (incl. space)
            if (input.length() >= 5 && input.length() < max_length) {
                // Split the input into separate strings on all spaces
                // The regex " +" will skip 1-or-more spaces
                names = input.split(" +");
                // We need to check if all names are valid
                // Start by ensuring there are at least two names in the array
                boolean allNamesValid = names.length >= 2;
                if (allNamesValid) {
                    // Loop over all names in the array
                    for (String name: names) {
                        // Update allNamesValid with the truth-value of the next name
                        allNamesValid = allNamesValid && name.chars().allMatch(Character::isAlphabetic);
                    }
                }
                valid = allNamesValid;
            }
            if (!valid){
                System.out.print("Not a valid name. Please try again: ");
            }
        }
        String fullName = Arrays.stream(names).collect(Collectors.joining(" "));
        return fullName;
    }
    // Just stick to the format for this exercise, don't bother with Mod11
    public static String getCPR() {
        boolean valid = false;
        String input = "";
        System.out.print("Please enter the patients cpr: ");
        while (!valid) {
            input = inputscanner.nextLine();
            if (input.trim().matches("^[0-9]{6}-[0-9]{4}$")) {
                valid = true;
            }
            else {
                System.out.print("Not a valid cpr. Please try again: ");
            }
        }
        return input.trim();
    }

    // Method to get a (simplified) address (Not part of the assignment!)
    // We assume the format is:
    // "streetName XX, PPPP cityname" where XX is the number of the street and PPPP is a postal code.
    // Could be extended to handle floors and apartment numbers as well
    // Does not handle streetnumbers of type ddX, where dd are digits and X is a letter (e.g. 11b or 17c)
    public static String getAddress() {
        boolean valid = false;
        String input = "";
        String streetName = "";
        String streetNumber = "";
        String postcode = "";
        String cityName = "";
        System.out.println("Please enter the patients address.");
        while (!valid) {
            // Grab the 4 needed strings from the user
            System.out.print("Street: ");
            streetName = inputscanner.nextLine().trim();
            System.out.print("Street number: ");
            streetNumber = inputscanner.nextLine().trim();
            System.out.print("Postal code: ");
            postcode = inputscanner.nextLine().trim();
            System.out.print("City: ");
            cityName = inputscanner.nextLine().trim();

            // streetname must be at least two alphabetic characters long
            valid = streetName.length() >= 2 && streetName.chars().allMatch(Character::isAlphabetic);
            // Streetnumber must be a number
            valid = valid && streetNumber.chars().allMatch(Character::isDigit);
            // Postal code must be exactly 4 digits
            valid = valid && postcode.chars().allMatch(Character::isDigit);
            // City must be must be at least two alphabetic characters long
            valid = valid && cityName.length() >= 2 && cityName.chars().allMatch(Character::isAlphabetic);

            if (!valid) {
                System.out.print("Not a valid Address. Please try again: ");
            }
        }
        String fullAddress = streetName + " " + streetNumber + ", " + postcode + " " + cityName;
        return fullAddress;
    }
    // Method for calculating a patient's age from their CPR-number
    public static int getAge(String cpr) {
        // Create a custom datetimeformatter for the cpr string, so we can calculate age.
        DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("ddMMuu");
        LocalDate today = LocalDate.now();
        LocalDate birthdate = LocalDate.parse(cpr.substring(0,6), dateFormatter);
        // If the birthdate is after today, it's probably in the 1900's. Subtract 100 years.
        if (birthdate.isAfter(today)) { birthdate = birthdate.minusYears(100); }
        int age = Period.between(birthdate, today).getYears();
        return age;
    }
    public static boolean genderIsFemale(String cpr) {
        // Grab the last digit of the cpr-string, convert it to an integer
        int lastDigitInCpr = Integer.parseInt(cpr.substring(9));
        // If the last digit is an even number, cpr-assigned gender is female
        return lastDigitInCpr % 2 == 0;
    }

    public static void main(String[] args) {
	    // Create a scanner that reads from STDIN
        inputscanner = new Scanner(System.in);
        // Fields for the data we need to collect
        String name;
        String cpr;
        String address;

        System.out.println("Please enter the patients name, cpr-number and address.");
        // Use the getName method to obtain the patient's name
        name = getName();
        System.out.print("CPR: ");
        // Use the getCPR method to obtain the patient's CPR-number
        cpr = getCPR();
        // Use the getAddress method to obtain the patient's address
        address = getAddress();
        System.out.println("The patients name: " + name +", cpr: " + cpr + ", address: " + address);
        int age = getAge(cpr);
        boolean assignedGenderIsFemale = genderIsFemale(cpr);
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
