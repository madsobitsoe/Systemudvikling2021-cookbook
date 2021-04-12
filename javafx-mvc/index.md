# MVC pattern with JavaFX - short intro

Read more here: https://docs.oracle.com/javafx/2/get_started/fxml_tutorial.htm

A Model View Controller pattern in JavaFX can be implemented by moving the GUI (View) definition to an FXML file.
The Controller part will handle the initialization of the GUI and all the input handling and update of the Model.

The Model is in this case the PatientRegister and "everything" around it :smile:

The "linking" between the FXML file and the controller is done via the following markups in the fxml file: 

* unique id's  e.g. ```java fx:id="cprText"```
* onAction's e.g. ```java onAction="#onAddPatient```

I will return to those later.

An FXML document looks like the following

```XML
<?xml version="1.0" encoding="UTF-8"?>
<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>
<?import javafx.scene.text.Font?>
<GridPane fx:id="grid" alignment="center" hgap="10" vgap="10" xmlns:fx="http://javafx.com/fxml/1"
          xmlns="http://javafx.com/javafx/11.0.1" fx:controller="sample.Controller"
        style="-fx-background-image:
        url(https://media.gettyimages.com/photos/these-hands-will-take-care-of-you-picture-id855467380?s=2048x2048)">
    <Label fx:id="cprLabel" text="CPR number" GridPane.columnIndex="0" GridPane.rowIndex="0" /> <!-- a comment -->

    <TextField fx:id="cprText"  prefWidth="200" promptText="######-####" GridPane.columnIndex="1" GridPane.rowIndex="0" />
    <Label fx:id="labelFirstName" text="First Name" GridPane.columnIndex="0" GridPane.rowIndex="1" />
    <TextField fx:id="firstNameText"  prefWidth="200" promptText="First Name" GridPane.columnIndex="1" GridPane.rowIndex="1" />
    <Label fx:id="labelLastName" text="Last Name" GridPane.columnIndex="2" GridPane.rowIndex="1" />
    <TextField fx:id="lastNameText" prefWidth="400" promptText="Last Name" GridPane.columnIndex="3" GridPane.rowIndex="1" />
    <Label fx:id="labelPhone" text="Phone" GridPane.columnIndex="0" GridPane.rowIndex="2" />
    <TextField fx:id="phoneText" prefWidth="200" promptText="+### ########" GridPane.columnIndex="1" GridPane.rowIndex="2" />
    <Label fx:id="labelEmail" text="Email" GridPane.columnIndex="2" GridPane.rowIndex="2" />
    <TextField fx:id="emailText" prefWidth="400" promptText="Email" GridPane.columnIndex="3" GridPane.rowIndex="2" />
    <TextArea fx:id="patientListTextArea" prefWidth="830" prefHeight="190"
              editable="false" GridPane.columnIndex="0" GridPane.rowIndex="5" GridPane.columnSpan="4" GridPane.rowSpan="1"
              text="Do nit be alarmed!\rThis massage is made bu a machine">

        <font>
            <Font name="Courier New" size="11.0" />
        </font>
    </TextArea>
    <Button fx:id="buttonAdd" text="Add patient" GridPane.columnIndex="0" GridPane.rowIndex="3" onAction="#onAddPatient"/>
    <Button fx:id="buttonClearAll" text="Clear all" GridPane.columnIndex="0" GridPane.rowIndex="6" onAction="#onClearAll"/>

   <columnConstraints>
      <ColumnConstraints />
      <ColumnConstraints />
      <ColumnConstraints />
      <ColumnConstraints />
   </columnConstraints>
   <rowConstraints>
      <RowConstraints />
      <RowConstraints />
      <RowConstraints />
   </rowConstraints>

</GridPane>
```

The first line describes the document type and encoding
The next three lines (import statements) are needed for various types of elements in you GUI. Don't worry: Intellij will warn you about
missing import and help you with the import of the right libraries.

Then follows the root element (the GridPane). The ```xml fx:controller="sample.Controller"``` statement links the fxml file to the controller class.
Make sure this is correct in your case (if your package is called week8, then the controller will most likely be something like week8.Controller instead of sample.Controller)

The next lines are children of the root element and definitions of the nodes (the elements) in your JavaFX scene.
Previously you wrote them directly as java statements in your Main class.

The FXML syntax is not java but xml. Don't worry - you will quickly learn - especially if you have been playing with html and css.

Example

```java
        Label cprLabel = new Label("CPR number");      // create a label displaying "CPR number"
		TextField cprText = new TextField();
        cprText.setPromptText("######-####");
		grid.add(cprLabel, 0, 0);
		grid.add(cprText, 1, 0);
		Button buttonAdd = new Button("Add patient");
		grid.add(buttonAdd, 0, 3);
```
is in FXML desribed as:

```XML
    <Label fx:id="cprLabel" text="CPR number" GridPane.columnIndex="0" GridPane.rowIndex="0" />
	<TextField fx:id="cprText"  prefWidth="200" promptText="######-####" GridPane.columnIndex="1" GridPane.rowIndex="0" />
	<Button fx:id="buttonAdd" text="Add patient" GridPane.columnIndex="0" GridPane.rowIndex="3" onAction="#onAddPatient"/>
```

The Main class can be reduced to loading the FXML file and defining and showing the stage - example:

```java
package sample;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.stage.Stage;

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) throws Exception {     // a Stage is the main window for a JavaFX application
        // initialize some test data

        Parent root = FXMLLoader.load(getClass().getResource("sample.fxml"));
        primaryStage.setTitle("Clinic App");
        Scene scene = new Scene(root, 1200, 500); // create a scene window 1200 x 500 pixels

        primaryStage.setScene(scene);                   // add the scene to the stage / application window
        primaryStage.show();                 // display the stage - important! otherwise nothing happens :-)

    }
    public static void main(String[] args) {
        launch(args);
    }

}

```



The Controller class that is "linked" to the FXML document needs to declare all the elements that you define in the FXML file:

```java
public class Controller {
    public Label cprLabel;
    public TextField cprText;
    public Label labelFirstName;
    public TextField firstNameText;
    public Label labelLastName;
    public TextField lastNameText;
    public Label labelPhone;
    public TextField phoneText;
    public TextField emailText;
    public Label labelEmail;
    public TextArea patientListTextArea;
    public Button buttonAdd;
    public Button buttonClearAll;
	.
	.
```

Hint:
If you need to refer to an element that is not instantiated by the Controller class (in our case the Gridpane will be indirectly instantiated by
the Main class) you will need to use the @FXML annotation. If you e.g. want to refer to the Gridpane defined in the FXML file shown above you will need
the following lines in you Controller:

```java
    @FXML
    private GridPane grid;
```

Note the special ```java public void initialize() ``` method. It will be called automatically at GUI initialization. 
So you can use it for initial creation of objects.

e.g.

```java

public void initialize() {
	Patient p1 = new Patient("Very","Ill","010101-0101");	
    p1.setPhoneNumber("+45 10203040");
    p1.setEmail("very.ill@homesick.org");
}
```

We finally need to link the GUI events (like mouse clicks) to the controller (The onAction events)


Without the FXML file you would write the actions like:

```java
buttonAdd.setOnMouseClicked(event -> {
            String cprInput= cprText.getText();
            String firstNameInput= capitalize(firstNameText.getText());
            String lastNameInput= capitalize(lastNameText.getText());
            String phoneInput = phoneText.getText();
            String emailInput = emailText.getText();
            boolean validInput= true;

            // validate the user input

            if (!DataValidator.isValidCPR(cprInput)){
                // we need a CPR number in order to register a user
                // highlight the CPR input field if content is invalid
                cprText.setStyle("-fx-background-color: yellow");
                validInput = false;
.
.
.
```

But with FXML in the scope you will instead write:

```java

public void onAddPatient(ActionEvent actionEvent) {
        String cprInput= cprText.getText();
        String firstNameInput= capitalize(firstNameText.getText());
        String lastNameInput= capitalize(lastNameText.getText());
        String phoneInput = phoneText.getText();
        String emailInput = emailText.getText();
        boolean validInput= true;

        // validate the user input

        if (!DataValidator.isValidCPR(cprInput)){
            // we need a CPR number in order to register a user
            // highlight the CPR input field if content is invalid
            cprText.setStyle("-fx-background-color: yellow");
            validInput = false;
.
.
.
.
```

Note that we have this way linked```xml onAction="#onAddPatient"``` to the ```java public void onAddPatient(ActionEvent actionEvent)``` method!

I hope you get the point. Full example will follow later (solution to Week8)
















