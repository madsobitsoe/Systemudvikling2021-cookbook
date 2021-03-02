# Solutions to Exercises week 4

Answers and padlet for the conceptual exercise can be found on Absalon

## Programming Exercise - task 1

*A small health clinic needs to have control over the employees' days so calendars do not allow patients to perform bookings during employees´ holiday time. Create a program that allows a patient to book appointments in a clinic. First, store the list of holidays in the clinic (you can do so by using, for instance, an arraylist). Then receive as an input the patient wishes for an appointment, and control that such date is not in the dates of holidays of the clinic.  Ensure that your program works with multiple days off for one employee.*

We solve the task with the following interpretation of the assigned task:
A clinic has clinic holidays ('we are closed in week 28 and 29') and all the employees have individual holidays as well.
So we will need a class to handle holidays and some classes to handle clinic and and employees.

Create a default Command line Java project.
Then create three class files: Holidays, Clinic and Employee.

Content of the *Holiday* class, which create a list of holidays and add methods to add and remove holidays to the list, check if a holiday i on the list and finally a method to print the holidays:

```java

package com.company;

import java.time.LocalDate;
import java.util.ArrayList;

public class Holidays {
    public ArrayList<LocalDate> holidays;
    public Holidays() {
        this.holidays = new ArrayList<LocalDate>();
    }
    public void addHoliday(LocalDate holiday) {
        if (! this.holidays.contains(holiday)) {this.holidays.add(holiday);}
    }
    public void removeHoliday(LocalDate holiday) {
        if (this.holidays.contains(holiday)) {this.holidays.remove(holiday);}
    }

    public Boolean containsDay(LocalDate holiday) {
        return this.holidays.contains(holiday);
    }

    public void printHolidays() {
        System.out.println("Number of holidays: " + this.holidays.size());
        for (LocalDate d : this.holidays) {
            System.out.println(d);
        }
    }
}
```

Content of the *Employee* class, which holds the name of the employee and the associated list of holidays:

```java
package com.company;
import java.time.LocalDate;

public class Employee {
    String name;
    Holidays holidays;
    public Employee(String inputName, Holidays inputHolidays) {
        this.name = inputName;
        this.holidays = inputHolidays;
    }
    public String checkAvailable(Clinic clinic, LocalDate bookingDate){
        if (clinic.holidays.containsDay(bookingDate)) {
            return ("The clinic is closed on " + bookingDate);
        } else if (this.holidays.containsDay(bookingDate)) {
            return("Employee is on holiday on " + bookingDate);
        } else {
            return("Employee is available on " + bookingDate);
        }
    };
}
```

Content of the *Clinic* class which hold the name of the clinic and a list of clinic holidays
```java
package com.company;

public class Clinic {
    Integer clinicNumber;
    Holidays holidays;
    public Clinic(Integer clinicNumber, Holidays inputHolidays) {
        this.clinicNumber = clinicNumber;
        this.holidays = inputHolidays;
    }

}
```

Finally the *Main* class in which we instantiate som test dates, two lists of dates (one for the clinic and one for an employee) and add and remove dates to the holiday lists.
Then we instantiate an employee and a clinic. (Ideally the employee should have been on a list of employees in the clinic class, but this is up to the reader to implement).
We use the methods to print the employees holidays and the clinic holidays to the console.
Finally we emulate some booking attempts. The program can be enhanced to take input from the console (use lessons learned from week 1).

```java
package com.company;

import java.time.LocalDate;

public class Main {

    public static void main(String[] args) {
	// write your code here
        LocalDate d1 = LocalDate.of(2021,2,22);
        LocalDate d2 = LocalDate.of(2021,2,23);
        LocalDate d3 = LocalDate.of(2021,2,23);
        LocalDate d4 = LocalDate.of(2021,2,24);


        Holidays emp1Holidays = new Holidays();
        Holidays clinicHolidays = new Holidays();
        emp1Holidays.addHoliday(d1);
        emp1Holidays.addHoliday(d2);
        emp1Holidays.addHoliday(d4);

        Employee emp1 = new Employee("Hans Nielsen",emp1Holidays);
        System.out.println("Employee holidays:");
        emp1.holidays.printHolidays();

        clinicHolidays.addHoliday(d1);
        clinicHolidays.addHoliday(d2);
        clinicHolidays.removeHoliday(d3);
        clinicHolidays.addHoliday(d4);
        Clinic clinic1 = new Clinic(01,clinicHolidays);

        System.out.println("Clinic holidays:");
        clinic1.holidays.printHolidays();

        // Emulating Booking Attempt
        LocalDate bookingDate= LocalDate.of(2021,2,23);
        System.out.println("Attempt: Booking employee on "+ bookingDate);
        System.out.println(emp1.checkAvailable(clinic1,bookingDate));

        bookingDate= LocalDate.of(2021,2,24);
        System.out.println("Attempt: Booking employee on "+ bookingDate);
        System.out.println(emp1.checkAvailable(clinic1,bookingDate));

        bookingDate= LocalDate.of(2021,2,19);
        System.out.println("Attempt: Booking employee on "+ bookingDate);
        System.out.println(emp1.checkAvailable(clinic1,bookingDate));


    }
}
```

Running the program should result in the following output to the console:

```

Employee holidays:
Number of holidays: 3
2021-02-22
2021-02-23
2021-02-24
Clinic holidays:
Number of holidays: 2
2021-02-22
2021-02-24
Attempt: Booking employee on 2021-02-23
Employee is on holiday on 2021-02-23
Attempt: Booking employee on 2021-02-24
The clinic is closed on 2021-02-24
Attempt: Booking employee on 2021-02-19
Employee is available on 2021-02-19
```

## Programming Exercise - task 2 and task 3

*Create a graphical user interface for the appointment booking task in the previous exercise.*

*Create a program that collects errors from inputs using try-catch statements. When the user enters the wishes for an appointment, he/she might input dates using different formats (for instance "4/10/21" and "Fourth of July". Ensure that the input corresponds to the format dd/mm/yyyy. Use exceptions to provide feedback to the user in case there was an error with the input received.*

The solution to both tasks as a whole is shown below:

Create a JavaFX project (remember to add the JavaFX SDK libs and the vm-options as mentioned here in the [javafx](https://madsobitsoe.github.io/Systemudvikling2021-cookbook/javafx/) secion of this cookbook

Copy the Holidays, Employee and Clinic classes into the new project.

We introduce a new Class *DataValidator* with the following content:

```java
package sample;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;

public class DataValidator {
    public static boolean isValidDate(String dateString) {
        Boolean result = false;

        try {
            DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
            LocalDate b = LocalDate.parse(dateString, dateFormatter);
            return true;
        }
        catch (DateTimeParseException dtpe) {
            System.err.println("Illegal date:"+dateString);
            return false;
        }

    }

}
```

The class is able to validate that the date is in the form "dd/MM/yyyy" - the solution to task 3 :-)

OK... We haven't made the GUI yet. For this we will need to change the content of Main (for more complex programming tasks we might want to put the GUI part in a seperate Class, but let us leave that for now...)

We change the content of the Main class to:

```java
package sample;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.stage.Stage;
import javafx.scene.layout.GridPane;
import javafx.geometry.Pos;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class Main extends Application {
    // We declare the "Text field/area" here in order to be able to use them in the clearFields method;
    TextField textBookingDate = new TextField();  // a TextField is a single lined field
    TextArea resultField = new TextArea();      // a TextArea is a multiline text field - more advanced than TextField


    @Override
    public void start(Stage primaryStage) {     // a Stage is the main window for a JavaFX application
        // initialize some test data
        LocalDate d1 = LocalDate.of(2021,2,22);
        LocalDate d2 = LocalDate.of(2021,2,23);
        LocalDate d3 = LocalDate.of(2021,2,23);
        LocalDate d4 = LocalDate.of(2021,2,24);

        Holidays emp1Holidays = new Holidays();
        Holidays clinicHolidays = new Holidays();

        emp1Holidays.addHoliday(d1);
        emp1Holidays.addHoliday(d2);
        emp1Holidays.addHoliday(d4);

        Employee emp1 = new Employee("Hans Nielsen",emp1Holidays);
        System.out.println("Employee holidays:");
        emp1.holidays.printHolidays();

        clinicHolidays.addHoliday(d1);
        clinicHolidays.addHoliday(d2);
        clinicHolidays.removeHoliday(d3);
        clinicHolidays.addHoliday(d4);
        Clinic clinic1 = new Clinic(21,clinicHolidays);

        System.out.println("Clinic holidays:");
        clinic1.holidays.printHolidays();

        // Emulating Booking Attempt on the console
        LocalDate bookingDate= LocalDate.of(2021,2,23);
        System.out.println("Attempt: Booking employee on "+ bookingDate);
        System.out.println(emp1.checkAvailable(clinic1,bookingDate));

        bookingDate= LocalDate.of(2021,2,24);
        System.out.println("Attempt: Booking employee on "+ bookingDate);
        System.out.println(emp1.checkAvailable(clinic1,bookingDate));

        bookingDate= LocalDate.of(2021,2,19);
        System.out.println("Attempt: Booking employee on "+ bookingDate);
        System.out.println(emp1.checkAvailable(clinic1,bookingDate));

        // Finished initialization of test values
        // Now let us create a GUI


        primaryStage.setTitle("Book "+ emp1.name + " on clinic " + clinic1.clinicNumber);   // set the title shown int th title bar
        GridPane grid = new GridPane();         // create a GridPane for a nice even flexible screen layout
        grid.setAlignment(Pos.CENTER);          // Align the grid to the center of the application window
        Label bookingLabel = new Label("Booking Date: ");      // create a text label displaying "CPR"
        grid.add(bookingLabel, 0, 0);              // put the newly created label in the top left corner of the grid (0,0)

        grid.setHgap(10);                       // set the horizontal gap between the fields to 10 pixels
        grid.setVgap(10);                       // set the vertical gap between the fields to 10 pixels
        //grid.setGridLinesVisible(true);       // nice if you want to check the grid layout

        textBookingDate.setPromptText("Enter a Booking Date");   // display some useful text to the user
        textBookingDate.setFocusTraversable(false);   // needed to be able to display the initial PromptText
        textBookingDate.setMaxWidth(150);         // Make sure the CPR field doesn't get too wide

        textBookingDate.setTooltip(new Tooltip("Booking date must be in the format dd/mm/yyyy")); // mouseover tooltip
        resultField.setPromptText("Do nit be alarmed!\rThis massage is made bu a machine");
        resultField.setPrefHeight(100);         // set the preferable height of the result field
        resultField.setPrefWidth(300);          // set the preferable width of the result field
        resultField.setEditable(false);         // make sure the user can't edit the result field
        resultField.setWrapText(true);          // ensure the text can be wrapped in the result field
        Button buttonOK = new Button("OK");          // create a clickable button
        Button buttonClearAll = new Button("Clear All");    // create another clickable button
        grid.add(textBookingDate, 1, 0);              // put the newly created label in the top left corner of the grid (0,0)

        // now create an event handler which will be called when the button is clicked
        buttonOK.setOnMouseClicked(event -> {
                // hear we check the input against a regular expression - we could
                // also choose the call an input validator (which we created in Week 2)
                // This one looks pretty hectic but basically checks if the first 6 numbers forms a valid date
                // and they are follow by a '-' and then by 4 digits.

                if (!DataValidator.isValidDate(textBookingDate.getText())) {
                    // when it doesn't match the pattern (dd/mm/yyyy)
                    // display a warning to the user
                    resultField.setText("Date is not in the right format");
                } else {
                    // we have a valid input - now check if the date is available
                    String text = textBookingDate.getText();
                    DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
                    String availability = emp1.checkAvailable(clinic1,LocalDate.parse(text, dateFormatter));
                    resultField.setText(availability);
                }
        });

        buttonClearAll.setOnMouseClicked(event -> {
                clearFields();  // call the method which clears all fields (see below)
        });

        // Now place the remaining fields in the grid

        //grid.add(textBookingDate, 1, 0);  // second column, first row
        grid.add(buttonOK, 2, 0);    // third column, first row
        grid.add(resultField, 1, 1);    // first column, second row
        grid.add(buttonClearAll, 2, 4); // third column, fifth row



        Scene scene = new Scene(grid, 600, 400); // create an scene window 600 x 400 pixels
        primaryStage.setScene(scene);                   // add the scene to the stage / application window
        primaryStage.show();                 // display the stage - important! otherwise nothing happens :-)
    }

    public void clearFields() { // method to clear text fields
        textBookingDate.clear();
        resultField.clear();
    }

    public static void main(String[] args) {
        launch(args);
    }

}
```

Explanations to the various part is given in the line comments and in the [javafx](https://madsobitsoe.github.io/Systemudvikling2021-cookbook/javafx/) section.

You can play with the grid posisitions and the Scene size (now set to 600,400). E.g.g What happens if you don't set it?
You can see the grid lines by uncommenting the line `java //grid.setGridLinesVisible(true);       // nice if you want to check the grid layout`
You can alter the existing nodes (TextField's TextArea, Labels and Buttons), add extra buttons, text fields, and text areas or whatever in order to create you own GUI

