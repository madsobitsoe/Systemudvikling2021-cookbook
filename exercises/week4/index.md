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
- **Group**: A group of nodes, i.e. content.

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
        // Create a group-object
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
