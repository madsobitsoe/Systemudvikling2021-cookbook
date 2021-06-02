#Marshal help (XML)

In the following I will provide an example on how to marshal (generate XML files from java objects) and
unmarshal (generate java objects from XML files) and get it to work in an Intellij IDEA project.

The example is partly inspired by https://stackabuse.com/reading-and-writing-xml-in-java,
partly by https://howtodoinjava.com/jaxb/solved-exception-in-thread-main-com-sun-xml-internal-bind-v2-runtime-illegalannotationsexception-3-counts-of-illegalannotationexceptions/
and partly by hard work.

## Installation and project integration

The "original" java.xml.bind library is no longer part of the standard jdk, so you must do a little extra in order to
get it work in your project in Intellij:

On of the new implementations is called jakarta XML binding https://eclipse-ee4j.github.io/jaxb-ri/ an can be downloaded from 
https://repo1.maven.org/maven2/com/sun/xml/bind/jaxb-ri/3.0.0/jaxb-ri-3.0.0.zip

more about jakarta XML binding and JAXB here: https://en.wikipedia.org/wiki/Jakarta_XML_Binding

Download and extract the zip file to some location (e.g. near the place where you have you javafx libraries)

In order to integrate it to you projekt so that you can make use of the functions you will need to (in IntelliJ) go to
project structure -> project settings -> Modules and in you current module (in my case xmlmarshal), select Dependencies and click on "+" and
select 1. JARS or directories.

Browse to the extrated directory of the jaxb-ri-3.0.0.zip fil that you have downloaded - all the way into the mod directory.
Select all the jar files and press ok.
You should now see something like:

next press OK and you are ready to import the jakarta classes and do the marshal operations:

## The example

The code will import (unmarshal) an xml file with the following content.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<person>
    <age>31</age>
    <hobbies>
        <element>Football</element>
        <element>Swimming</element>
    </hobbies>
    <isMarried>true</isMarried>
    <kids>
        <person>
            <age>5</age>
            <name>Billy</name>
        </person>
        <person>
            <age>3</age>
            <name>Milly</name>
        </person>
    </kids>
    <name>Benjamin Watson</name>
</person>
```

then write the person objects to the console and finally write an xml file (which you yourself can check against the input file)

the console output:

```
"C:\Program Files\Java\jdk-15.0.2\bin\java.exe" "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2020.3.2\lib\idea_rt.jar=52190:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2020.3.2\bin" -Dfile.encoding=UTF-8 -classpath C:\Users\ja\IdeaProjects\xmlmarshal\out\production\xmlmarshal;C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\mod\jakarta.activation.jar;C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\mod\jakarta.xml.bind-api.jar;C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\mod\jaxb-core.jar;C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\mod\jaxb-impl.jar;C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\mod\jaxb-jxc.jar;C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\mod\jaxb-xjc.jar com.company.Solution
Person{name='Benjamin Watson', age=31, isMarried=true, hobbies=[Football, Swimming], kids=[Person{name='Billy', age=5, isMarried=null, hobbies=null, kids=null}, Person{name='Milly', age=3, isMarried=null, hobbies=null, kids=null}]}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<person>
    <age>31</age>
    <hobbies>
        <element>Football</element>
        <element>Swimming</element>
    </hobbies>
    <isMarried>true</isMarried>
    <kids>
        <person>
            <age>5</age>
            <name>Billy</name>
        </person>
        <person>
            <age>3</age>
            <name>Milly</name>
        </person>
    </kids>
    <name>Benjamin Watson</name>
</person>

Process finished with exit code 0

```






The program is created in IntelliJ as a simple Java console application and consists of the main class - here called Solution and a Person class.

Let us first have a look at the Person class

```java
package com.company;
import jakarta.xml.bind.annotation.*;
import java.util.List;

@XmlRootElement
//Optional: set the order of the elements
@XmlType(propOrder={"age", "hobbies","married","kids","name"})
public class Person {

    public Person() {
        this.name = null;
        this.age = null;
        this.isMarried = null;
        this.hobbies = null;
        this.kids = null;
    }

    public Person(String name, int age, Boolean isMarried,
                  List<String> hobbies, List<Person> kids) {
        this.name = name;
        this.age = age;
        this.isMarried = isMarried;
        this.hobbies = hobbies;
        this.kids = kids;
    }


    private String name;
    private Integer age;
    private Boolean isMarried;
    private List<String> hobbies;
    private List<Person> kids;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Boolean isMarried() {
        return isMarried;
    }

    @XmlElement(name = "isMarried")
    public void setMarried(Boolean married) {
        isMarried = married;
    }

    @XmlElementWrapper(name = "hobbies")
    @XmlElement(name = "element")
    public List<String> getHobbies() {
        return hobbies;
    }

    public void setHobbies(List<String> hobbies) {
        this.hobbies = hobbies;
    }

    public List<Person> getKids() {
        return kids;
    }

    @XmlElementWrapper(name = "kids")
    @XmlElement(name = "person")
    public void setKids(List<Person> kids) {
        this.kids = kids;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", isMarried=" + isMarried +
                ", hobbies=" + hobbies +
                ", kids=" + kids +
                '}';
    }
}
```

The import statement ```java import jakarta.xml.bind.annotation.*;``` enable us to annotate the attributes in the Person class so that
the marshaller method (in the main class) knows what attributes shall translate to what XML elements.

The root element (person) is annotated by ```@XmlRootElement```. I have also put in the order of the elements: ```@XmlType(propOrder={"age", "hobbies","married","kids","name"})```, but that is optional.

Then each element is annotated by ```@XmlElement```. Note that lists need a ```@XmlElementWrapper``` in order to be exported correctly.


The main class Solution follows here:

```java
package com.company;
import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.Marshaller;
import jakarta.xml.bind.Unmarshaller;
import java.io.File;

public class Solution {
    public static void main(String[] args) throws Exception {
        Person person = XMLtoPersonExample("person.xml");
        System.out.println(person);
        personToXMLExample("person-output.xml", person);
    }

    private static Person XMLtoPersonExample(String filename) throws Exception {
        File file = new File(filename);
        JAXBContext jaxbContext = JAXBContext.newInstance(Person.class);

        Unmarshaller jaxbUnmarshaller = jaxbContext.createUnmarshaller();
        return (Person) jaxbUnmarshaller.unmarshal(file);
    }

    private static void personToXMLExample(String filename, Person person) throws Exception {
        File file = new File(filename);
        JAXBContext jaxbContext = JAXBContext.newInstance(Person.class);

        Marshaller jaxbMarshaller = jaxbContext.createMarshaller();

        jaxbMarshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);

        jaxbMarshaller.marshal(person, file);
        jaxbMarshaller.marshal(person, System.out);
    }
}
```

The class imports three classes used for the XML operations.

The abstract class JAXBContext provides the "link" between the java class that we annotated and the XML file. 

*@Mads: Please comment here!*

The Marshall and Unmarshaller classes will do actual work of reading and writing the xml strings from the java class.

I have placed the input xml file "person.xml" and the output xml files directly in the project top folder. You may want to change the path in your project.


## xml validation

The XML file is following a schema defined by

```xsd
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<xs:schema version="1.0" xmlns:xs="http://www.w3.org/2001/XMLSchema">

  <xs:element name="person" type="person"/>

  <xs:complexType name="person">
    <xs:sequence>
      <xs:element name="age" type="xs:int"/>
      <xs:element name="hobbies" minOccurs="0">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="element" type="xs:string" minOccurs="0" maxOccurs="unbounded"/>
          </xs:sequence>
        </xs:complexType>
      </xs:element>
      <xs:element name="isMarried" type="xs:boolean" minOccurs="0"/>
      <xs:element name="kids" minOccurs="0">
        <xs:complexType>
          <xs:sequence>
            <xs:element ref="person" minOccurs="0" maxOccurs="unbounded"/>
          </xs:sequence>
        </xs:complexType>
      </xs:element>
      <xs:element name="name" type="xs:string" minOccurs="0"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


The schema is generated using the script schemagen.bat (windows) or schemagen.sh (mac/linux)  in the downloaded and extraced jaxb-ri-3.0.0 structure
The script takes the java class as argument (in this case Person.java)

Basically the schema tells us that an xml file will consists of a list of elements named person which again contains elements named age,
hobbies, isMarried, kids and name
Hobbies is actually a list (0 or more elements) of strings kids is a list (0 or more elements) of person.



Windows example:

```
C:\Users\ja\IdeaProjects\xmlmarshal\src\com\company>C:\Users\ja\javaSDK\jaxb-ri-3.0.0\jaxb-ri\bin\schemagen.bat Person.java
Java major version: 15
Note: Writing C:\Users\ja\IdeaProjects\xmlmarshal\src\com\company\schema1.xsd
```

The schema can be used during the unmarshaller process in order to validated the xml file and check if it follows the right structure and has the right elements.
But that is for another cookbook recipie.











