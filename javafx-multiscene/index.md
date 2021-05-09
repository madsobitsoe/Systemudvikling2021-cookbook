


# Quick JavaFX Multi Scene Howto

This is a quick instruction in how you can create a JavaFX application with multiple controllers and multiple Scenes.
The scenes are swapped when menu items are clicked.

The application is roughly an extension to what we have created in the other exercises.
The classes have almost remained untouched - I think you can quickly find the changes if any.

The application will allow you to add (and list) new patients and to add an employee. Please notice that
their is no employee list - that is for you to create - if you haven't done so already. Maybe you can also add the holdidays part?

The main idea is to create a "main" controller and fxml file - here called MultiSceneController.java and Multiscene.fxml
I have created it using SceneBuilder and creating a new basic application from the templates.
Then I have removed (almost) all stuff I don't use.


The fxml file defines a basic menu and an empty anchor pane.
The idea is - depending on the context - to add and remove different grid panes to the anchor pane.

Each grid pane has it's own view part (fxml) and controller.


The switching is done in MultiSceneController by calling setGridPane method) (in Main) when a menu item is clicked.

You can easily yourself add the fxml and controller files for the "Consult" menu items.


## Main Class
```java
package sample;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Scene;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

import java.util.ArrayList;
import java.util.List;

public class Main extends Application {
    static VBox root;
    static List<GridPane> gridPaneList = new ArrayList<GridPane> ();
    private static int index_current = 0;
    @Override
    public void start(Stage primaryStage) throws Exception {     // a Stage is the main window for a JavaFX application
        // initialize some test data

        root = FXMLLoader.load(getClass().getResource("Multiscene.fxml"));
        gridPaneList.add(FXMLLoader.load(getClass().getResource("patients.fxml")));
        gridPaneList.add(FXMLLoader.load(getClass().getResource("employees.fxml")));

        //root.getChildren().add(gridPanePatients);
        primaryStage.setTitle("Clinic App");
        Scene scene = new Scene(root, 1200, 500); // create a scene window 1200 x 500 pixels

        primaryStage.setScene(scene);                   // add the scene to the stage / application window
        primaryStage.show();                 // display the stage - important! otherwise nothing happens :-)

    }
    public static void main(String[] args) {
        launch(args);
    }

    public static void setGridPane (int index) {
        root.getChildren().remove(gridPaneList.get(index_current));
        root.getChildren().add(gridPaneList.get(index));
        index_current=index;

    };

}
```

## Controller and fxml files for the menu

```java
package sample;

import javafx.event.ActionEvent;

public class MultisceneController {
    public void newPatient(ActionEvent actionEvent) {
        Main.setGridPane(0);
    }

    public void newEmployee(ActionEvent actionEvent) {
        Main.setGridPane(1);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.control.Label?>
<?import javafx.scene.control.Menu?>
<?import javafx.scene.control.MenuBar?>
<?import javafx.scene.control.MenuItem?>
<?import javafx.scene.control.SeparatorMenuItem?>
<?import javafx.scene.layout.AnchorPane?>
<?import javafx.scene.layout.VBox?>
<?import javafx.scene.text.Font?>

<!-- <VBox prefHeight="800.0" prefWidth="1200.0" xmlns="http://javafx.com/javafx/15.0.1" xmlns:fx="http://javafx.com/fxml/1"
-->
<VBox prefWidth="1200.0" xmlns="http://javafx.com/javafx/15.0.1" xmlns:fx="http://javafx.com/fxml/1"
      fx:controller="sample.MultisceneController">
  <children>
    <MenuBar VBox.vgrow="NEVER">
      <menus>
        <Menu mnemonicParsing="false" text="Patients">
          <items>
            <MenuItem mnemonicParsing="false" text="New" onAction="#newPatient" />
            <MenuItem mnemonicParsing="false" text="Consult" />
          </items>
        </Menu>
        <Menu mnemonicParsing="false" text="Employees">
          <items>
            <MenuItem mnemonicParsing="false" text="New" onAction="#newEmployee" />
            <MenuItem mnemonicParsing="false" text="Consult" />
          </items>
        </Menu>
        <Menu mnemonicParsing="false" text="Reports">
          <items>
            <MenuItem mnemonicParsing="false" text="Incidents by ZIP code" />
          </items>
        </Menu>
        <Menu mnemonicParsing="false" text="Help">
          <items>
            <MenuItem mnemonicParsing="false" text="About Clinic App" />
          </items>
        </Menu>
      </menus>
    </MenuBar>
    <!--
    <AnchorPane maxHeight="-1.0" maxWidth="-1.0" prefHeight="-1.0" prefWidth="-1.0" VBox.vgrow="ALWAYS">
    -->
    <AnchorPane>
    </AnchorPane>
  </children>
</VBox>
```



![](ClinicApp-main.png)
![](ClinicApp-newemp-menu.png)
![](ClinicApp-addingemp.png)
![](ClinicApp-newpatient-menu.png)
![](ClinicApp-newpatient-added.png)


A zip file of the src directory can be downloaded here: [multiscene.zip](multiscene.zip)



