# Solutions for exercises
- [Week 1](#week-1)


## Week 1
### Task 2
> Make a program that calculates the cost of an admission of X days length to the price Y per day.


#### Solution

```
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
