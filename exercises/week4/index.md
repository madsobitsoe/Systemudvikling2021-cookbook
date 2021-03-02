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



Follow the guide here: https://www.jetbrains.com/help/idea/javafx.html (or the detailed guide in the `Install Guide - Windows`-pdf, which you can find on absalon).
If you have not downloaded the JavaFX library, you will need to do so. (The guide also covers that).

The path you insert in the `VM options` field will depend on your system and where the JavaFX library is located.

On my Mac, the string I input was:

`--module-path "/Users/MyUserName/Documents/diku/systemUdvikling2021/javalibs/javafx-sdk-15.0.1/lib" --add-modules javafx.controls,javafx.fxml`

In general, you need

`--module-path "PathToJavaFX" --add-modules javafx.controls,javafx.fxml`
where you replace `"PathToJavaFX"` with the path to the `lib`-folder in the JavaFX-folder you extracted from the zip-file.


If done correctly, you should be able to run the program and see an empty window.


# The very short introduction to JavaFX

JavaFX works with a few different concepts, that are important to know.

- **Stage**: A window is called a `Stage` in JavaFX
- **Scene**: The *contents* of a `Stage` (window) is called a `Scene`
- **Scene Graph**: The *contents* of a `Stage` are organized into a `Scene Graph`
- **Node**: Some content we can put into a `Scene Graph`, e.g. a textbox, a button or some pretty images. Nodes can contain other nodes.
- **Event**: When the user clicks a button, drags the mouse, moves the window etc., it is an `Event`. We can detect events in our code and do different things when they happen.

These concepts will make more sense when we have used them.

## Making a very simple Graphical User Interface

Try copy the following code and inserting it into your javaFx project (and replacing everything that was already there).

It should create a window with a blue background and some black text.
Try changing the text, the position, the background color, the text size and the window size.
Try adding another text object as well.

```java
import javafx.application.Application;
import javafx.collections.ObservableList;
import javafx.scene.*;
import javafx.scene.paint.Color;
import javafx.scene.text.*;
import javafx.stage.Stage;

public class Main extends Application {
    @Override
    public void start(Stage primaryStage) throws Exception{
        primaryStage.setTitle("Hello JavaFX!");
        // Create a group-object. This is an "empty layout"
        Group root = new Group();
        // The ObservableList, is a list of all the things we add to the group.
        // It starts out empty, but we will add objects/nodes to it to display them in our window
        ObservableList list = root.getChildren();
        // Create a "Text" object that will display text
        Text text = new Text();
        // Set the font-size to 45
        text.setFont(new Font(45));
        // Set the x- and y-location of the text object in the window
        text.setX(50);
        text.setY(150);
        // Finally, set the text we want to display
        text.setText("Hello JavaFX!");
        // Add the text to the list of nodes in the "root"-group
        list.add(text);
        // Set the window-width to 400 and the window height to 300
        Scene scene = new Scene(root, 400, 300);
        // Add a background-color
        scene.setFill(Color.CORNFLOWERBLUE);
        // Set the current scene in the stage to the new scene we just created, only containing our root-object.
        primaryStage.setScene(scene);
        // Finally, show the stage!
        primaryStage.show();
    }
    public static void main(String[] args) {
        launch(args);
    }
}
```




## Making a more interesting user interface

First we post the full code listing, then we try to break down the individual parts.

``` java
package sample;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.stage.Stage;
import javafx.scene.layout.GridPane;
import javafx.geometry.Pos;

public class Main extends Application {
    // We declare the "Text field/area" here in order to be able to use them in the clearFields method;
    TextField textCprNumber = new TextField();  // a TextField is a single lined field
    TextArea resultField = new TextArea();      // a TextArea is a multiline text field - more advanced than TextField
    DataValidator dataValidator = new DataValidator(); // create and instance of the  DataValidator class from week 2
    @Override
    public void start(Stage primaryStage) {     // a Stage is the main window for a JavaFX application
        primaryStage.setTitle("Enter CPR number and hit the button");   // set the title shown int th title bar
        GridPane grid = new GridPane();         // create a GridPane for a nice even flexible screen layout
        grid.setAlignment(Pos.CENTER);          // Align the grid to the center of the application window
        Label CPR = new Label("CPR: ");      // create a text label displaying "CPR"
        grid.add(CPR, 0, 0);              // put the newly created label in the top left corner of the grid (0,0)
        grid.setHgap(10);                       // set the horizontal gap between the fields to 10 pixels
        grid.setVgap(10);                       // set the vertical gap between the fields to 10 pixels
        //grid.setGridLinesVisible(true);       // nice if you want to check the grid layout

        textCprNumber.setPromptText("Enter your CPR Number");   // display some useful text to the user
        textCprNumber.setFocusTraversable(false);   // needed to be able to display the initial PromptText
        textCprNumber.setMaxWidth(150);         // Make sure the CPR field doesn't get too wide
        textCprNumber.setTooltip(new Tooltip("CPR Number must be in the format ######-####")); // mouseover tooltip
        resultField.setPromptText("Enter a CPR number in the above field and click on 'Hit me'. This field will then show if the format of the CPR number can be validated");
        resultField.setPrefHeight(100);         // set the preferable height of the result field
        resultField.setPrefWidth(300);          // set the preferable width of the result field
        resultField.setEditable(false);         // make sure the user can't edit the result field
        resultField.setWrapText(true);          // ensure the text can be wrapped in the result field
        Button buttonHitMe = new Button("Hit me");          // create a clickable button
        Button buttonClearAll = new Button("Clear All");    // create another clickable button

        // now create an event handler which will be called when the button is clicked
        buttonHitMe.setOnMouseClicked(event -> {
                if (dataValidator.isValidCPR(textCprNumber.getText())) {
                    // we have a valid input which we display in the result field
                    String text = textCprNumber.getText();
                    resultField.setText("CPR number: " + text);
                }
                else {
                    // display a warning to the user
                    resultField.setText("CPR number is not in correct format");
                }
        });

        buttonClearAll.setOnMouseClicked(event -> {
                clearFields();  // call the method which clears all fields (see below)
        });

        // Now place the remaining fields in the grid
        grid.add(textCprNumber, 1, 0);  // second column, first row
        grid.add(buttonHitMe, 2, 0);    // third column, first row
        grid.add(resultField, 1, 1);    // first column, second row
        grid.add(buttonClearAll, 2, 4); // third column, fifth row

        Scene scene = new Scene(grid, 600, 400); // create an scene window 600 x 400 pixels
        primaryStage.setScene(scene);                   // add the scene to the stage / application window
        primaryStage.show();                 // display the stage - important! otherwise nothing happens :-)
    }

    public void clearFields() { // method to clear text fields
        textCprNumber.clear();
        resultField.clear();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

Phew, that is a lot of code. Let's break it down.


To begin with, we have almost the same structure as all our "main"-classes.

``` java
... imports here

public class Main extends Application {
    ...
    @Override
    public void start(Stage primaryStage) {     // a Stage is the main window for a JavaFX application
    ...
    }

    public void clearFields() { // method to clear text fields
    ...
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

We have a class, with the name `Main`. Our java file is called `Main.java`.

The first line after the imports is:

``` java
public class Main extends Application {
```

We have not covered inheritance in java/object-oriented, so a brief explanation:
`Application` is a class defined in JavaFX. It is the "main class" of a GUI made with JavaFX and will handle things like creating a a window for us.
The `extends` keyword, means that our class, `Main` **is a** `Application`, i.e. it *inherits* all the methods defined in Application. This allows us to reuse everything in the `Application` class, while *extending* it with whatever we need.
This part is not important to understand. The important point is, that whenever we create a GUI with JavaFX, the Main class of our program should *extend* the class `Application`.


The next two lines are:
``` java
TextField textCprNumber = new TextField();  // a TextField is a single lined field
TextArea resultField = new TextArea();      // a TextArea is a multiline text field - more advanced than TextField
```

JavaFX has defined a lot of nice building blocks for us, to design a GUI with.
Here we create two of them, a `TextField` where a user can input a single line of text, and a `TextArea`, where a user (or we) can input multiple lines of text.


Then we have the two lines:
``` java
    @Override
    public void start(Stage primaryStage) {     // a Stage is the main window for a JavaFX application
```

Remember that `Main` *extends* `Application`.
In the `Application` class, there is a method named `start`. When we launch our java program with a JavaFX GUI, the `start`-method is automatically called.
The `@Override` annotation means that we want to *override* the method in `Application`, or *reimplement* the method. We want to make our own version, so we can decide what is displayed in the window.

The `start`-method takes one argument, `primaryStage` of type `Stage`. A `Stage` is a window, that JavaFX creates for us, that we will put our `TextField`s, `TextArea`s and `Button`s into.


Next up, the following line:

``` java
primaryStage.setTitle("Enter CPR number and hit the button");   // set the title shown in the title bar
```
We use the `primaryStage` object (our window!) and call the `setTitle` method, which will set the window title to whatever string we put into it.
This will usually be the name of our program, i.e. `Chrome`, `Firefox`, `Discord`, `iTunes` etc.


Now we are finally getting to the interesting parts:
``` java
GridPane grid = new GridPane();         // create a GridPane for a nice even flexible screen layout
grid.setAlignment(Pos.CENTER);          // Align the grid to the center of the application window
Label CPR = new Label("CPR: ");      // create a text label displaying "CPR"
grid.add(CPR, 0, 0);              // put the newly created label in the top left corner of the grid (0,0)
```

So, the first line creates a new `GridPane` object. A `GridPane` is a layout, that allows us to specify where our `TextField`s, `TextArea`s and `Button`s will appear.
The second line sets the *alignment* of our new grid, to be at the center of our window.
The third line creates a `Label`. A `Label` can be used when we want to write text, that the user cannot edit.
The fourth line *adds* our new label to our `grid`, placing it at the top left corner.


So far so good.
The next lines:
``` java
textCprNumber.setPromptText("Enter your CPR Number");   // display some useful text to the user
textCprNumber.setFocusTraversable(false);   // needed to be able to display the initial PromptText
textCprNumber.setMaxWidth(150);         // Make sure the CPR field doesn't get too wide
textCprNumber.setTooltip(new Tooltip("CPR Number must be in the format ######-####")); // mouseover tooltip
```

These four lines will set up our `TextField` object. First we set the "prompt text", i.e. text that will appear until the user fills in the field.
The `setMaxWidth` method will set a maximum length (width) of our textfield.
The `setToolTip`-method allows us to display a helpful message if the user hovers their mouse over the field.


We then setup the `resultField`, our `TextArea`:

``` java
resultField.setPrefHeight(100);         // set the preferable height of the result field
resultField.setPrefWidth(300);          // set the preferable width of the result field
resultField.setEditable(false);         // make sure the user can't edit the result field
resultField.setWrapText(true);          // ensure the text can be wrapped in the result field
```
We define what height and width (in pixels) we prefer and make sure the user can not edit the field.
We use the `setWrapText`-method, to ensure a line that doesn't fit into the window will automatically be broken into multiple lines.


Next up, buttons!

``` java
Button buttonHitMe = new Button("Hit me");          // create a clickable button
Button buttonClearAll = new Button("Clear All");    // create another clickable button
```

`Button` is a JavaFX class that allows us to create a button, which the user can click. Very useful.
Here, we create two buttons. Next, we will decide what happens when we click them.


We start by looking at the simplest button - the `buttonClearAll`:

``` java
buttonClearAll.setOnMouseClicked(event -> {
        clearFields();  // call the method which clears all fields (see below)
});
```

What happens here?
The `Button` class can handle `Event`s. One such event is `onMouseClicked`. This `Event` is *fired* (happens) when the user clicks the button.
The `setOnMouseClicked`-method allows us to *attach* a method to that event. The method will then be called, whenever the user clicks the button.

In this case, we use a *lambda* method. A *lambda* method is simply a method that doesn't have a name and isn't tied to a class. We can use lambda-methods when we only need the method once, like here, when we need it only for the button.

The `event` is the parameter/argument to our lambda-method. In this case we don't use the event for anything.
Between the `{` and `}` we have the *body* of our method.
Here we just call another method, `clearFields()` (which will reset the text fields for us).

Let's look at the first button now:

``` java
// now create an event handler which will be called when the button is clicked
buttonHitMe.setOnMouseClicked(event -> {
        if (dataValidator.isValidCPR(textCprNumber.getText())) {
            // we have a valid input which we display in the result field
            String text = textCprNumber.getText();
            resultField.setText("CPR number: " + text);
        }
        else {
            // display a warning to the user
            resultField.setText("CPR number is not in correct format");
        }
});
```

We once again *attach* a *lambda method* to the `Button`.
In this method, we have an if-statement.
We use the `getText()`-method of the `textCprNumber` `TextField`, to get the String, typed in by the user.
We give that `String` as the argument to our `isValidCPR` method in the `DataValidator` class.

If the user did in fact input a valid CPR, we get the string they entered, and update the `TextArea` with the name `resultField`.
If not, we display a message in the `resultField` `TextArea` to the user, saying the CPR was invalid.

We are almost done!

``` java
grid.add(textCprNumber, 1, 0);  // second column, first row
grid.add(buttonHitMe, 2, 0);    // third column, first row
grid.add(resultField, 1, 1);    // first column, second row
grid.add(buttonClearAll, 2, 4); // third column, fifth row
```

These four lines will place our `TextField`, `TextArea` and two `Button`s in our `GridPane`.

The last lines of the `start`-method are:

``` java
    Scene scene = new Scene(grid, 600, 400); // create an scene window 600 x 400 pixels
    primaryStage.setScene(scene);                   // add the scene to the stage / application window
    primaryStage.show();                 // display the stage - important! otherwise nothing happens :-)
}
```

First, we create a `Scene` object, named `scene`. We give the `grid` as the *root node* and set the window size to 600 pixels width and 400 pixels height.
Next, we *attach* the `scene` to the `primaryStage`, i.e. we tell JavaFX that what we want to display in our *stage* (window), is the *Scene*. The *Scene* contains our gridpane, which contains the two buttons, the `TextField` and the `TextArea`.

Next up, the `clearFields`-method we used with one of the buttons.
``` java
public void clearFields() { // method to clear text fields
    textCprNumber.clear();
    resultField.clear();
}
```

It simply calls the `clear()` method of the `TextField` and `TextArea`.


Last, we have the `main`-method - where our program will start execution.

``` java
public static void main(String[] args) {
    launch(args);
}
```

Here, we call a method `launch`. The `launch` method is part of JavaFX, and will start our GUI-application for us, then call the `start` method we have written.
