# Setting up a JavaFX project.


Follow the guide here: https://www.jetbrains.com/help/idea/javafx.html (or the detailed guide in the `Install Guide - Windows`-pdf, which you can find on absalon).
If you have not downloaded the JavaFX library, you will need to do so. (The guide also covers that).

The path you insert in the `VM options` field will depend on your system and where the JavaFX library is located.

On my Mac, the string I input was:

`--module-path "/Users/MyUserName/Documents/diku/systemUdvikling2021/javalibs/javafx-sdk-15.0.1/lib" --add-modules javafx.controls,javafx.fxml`

In general, you need

``--module-path "PathToJavaFX" --add-modules javafx.controls,javafx.fxml`
where you replace `"PathToJavaFX"` with the path to the `lib`-folder in the JavaFX-folder you extracted from the zip-file.


If done correctly, you should be able to run the program and see an empty window.


# The very short introduction to JavaFX

JavaFX works with a few different concepts, that are important to know.

- **Stage**: A window is called a `Stage` in JavaFX
- **Scene**: The *contents* of a `Stage` (window) is called a `Scene`
- **Scene Graph**: The *contents* of a `Stage` are organized into a `Scene Graph`
- **Node**: Some content we can put into a `Scene Graph`, e.g. a textbox, a button or some pretty images.
- **Group**: A group of nodes, i.e. content. A group is used for layout in the GUI, e.g. in a grid view or a tile view.

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
                // here we check the input against a regular expression - we could
                // also choose the call an input validator (which we created in Week 2)
                // This one looks pretty hectic but basically checks if the first 6 numbers forms a valid date
                // and they are follow by a '-' and then by 4 digits.
                if (!textCprNumber.getText().matches("^(((0[1-9]|[12][0-9]|30)(0[13-9]|1[012])|31(0[13578]|1[02])|(0[1-9]|1[0-9]|2[0-8])02)[0-9]{2}|2902((([2468][048]|[02468][48])|[13579][26])|([13579][26]|[02468][048]|0[0-9]|1[0-6])00))-\\d{4}")) {
                    // when it doesn't match the pattern (######-####)
                    // display a warning to thes user
                    resultField.setText("CPR number is not in correct format");
                } else {
                    // we have a valid input which we display in the result field
                    String text = textCprNumber.getText();
                    resultField.setText("CPR number: " + text);
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
