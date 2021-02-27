# Visitor Design Pattern

## Overview

Your object structure inside an application may be complicated and varied. A good example is what could be created using the composite structure.

The objects that make up the hierarchy of objects, can be anything and most likely complicated to modify as your application grows. 

Instead, when designing the objects in your application that may be structured in a hierarchical fashion, you can allow them to implement a **Visitor** interface. 

The Visitor interface describes an `accept()` method that a different object, called a Visitor, will use in order to traverse through the existing object hierarchy and read the internal attributes of an object.

The Visitor pattern is useful when you want to analyze, or reproduce an alternative object hierarchy without implementing extra code in the object classes, except for the original requirements set by implementing the Visitor interface.

Similar to the template pattern it could be used to output different versions of a document but more suited to objects that may be members of a hierarchy.

## Terminology

* **Visitor Interface**: An interface for the Concrete Visitors.
* **Concrete Visitor**: The Concrete Visitor will traverse the hierarchy of elements.
* **Visitable Interface**: The interface that elements should implement which describes the `accept()` method that will allow them to be visited (traversed).
* **Concrete Element**: An object that will be visited. An application will contain a variable number of Elements than can be structured in any particular hierarchy.

## Visitor UML Diagram

![Visitor Pattern UML Diagram](/img/visitor_concept.svg)

## Source Code

In the concept code below, a hierarchy of any object is created. It is similar to a simplified composite. The objects of `Element` can also contain a hierarchy of sub elements.

The `Element` class could also consist of many variations, but this example uses only one.

Rather than writing specific code inside all these elements every time I wanted to handle a new custom operation, I can implement the `IVisitable` interface and create the `accept()` method which allows the Visitor to pass through it and access the Elements internal attributes.

Two different Visitor classes are created, `PrintElementNamesVisitor` and `CalculateElementTotalsVisitor` . They are instantiated and passed through the existing Object hierarchy using the same `IVisitable` interface.

## Output

``` bash
python ./visitor/visitor_concept.py  
D
B
C
A
561
```

## Visitor Example Use Case

In the example, the client creates a car with parts. 

The car and parts inherit an abstract car parts class with predefined property getters and setters. 

Instead of creating methods in the car parts classes and abstract class that run bespoke methods, the car parts can all implement the `IVisitor` interface.

This allows for the later creation of Visitor objects to run specific tasks on the existing hierarchy of objects.

## Visitor Example UML Diagram

![Visitor Pattern Use Case UML Diagram](/img/visitor_example.svg)

## Output

``` bash
python ./visitor/client.py
Utility     :ABC-123-21
V8 engine   :DEF-456-21
FrontLeft   :GHI-789FL-21
FrontRight  :GHI-789FR-21
BackLeft    :GHI-789BL-21
BackRight   :GHI-789BR-21
Total Price = 4132
```

## New Coding Concepts

### Instance `hasattr()`

In the Visitor objects in the example use case above, I test if the elements have a certain attribute during the visit operation.

``` python
def visit(cls, element):
    if hasattr(element, 'price'):
        ...
```

The `hasattr()` method can be used to test if an instantiated object has an attribute of a particular name.

``` python
class ClassA():
    name = "abc"
    value = 123

CLASS_A = ClassA()
print(hasattr(CLASS_A, "name"))
print(hasattr(CLASS_A, "value"))
print(hasattr(CLASS_A, "date"))

```

Outputs

``` bash
True
True
False
```

### String `expandtabs()`

When printing strings to the console, you can include special characters `\t` that print a series of extra spaces called tabs. The tabs help present multiline text in a more tabular form which appears to be neater to look at. 

``` bash
abc    123
defg   456
hi     78910
```

The number of spaces added depends on the size of the word before the `\t` character in the string. By default, a tab makes up 8 spaces.

Now, not all words separated by a tab will line up the same on the next line.

``` bash
abcdef  123
cdefghij        4563
ghi     789
jklmn   1011

```

The problem occurs usually when a word is already 8 or more characters long.

To help solve the spacing issue, you can use the `expandtabs()` method on a string to set how many characters a tab will use.

``` python
print("abcdef\t123".expandtabs(10))
print("cdefghij\t4563".expandtabs(10))
print("ghi\t789".expandtabs(10))
print("jklmn\t1011".expandtabs(10))
```

Now outputs

``` bash
abcdef    123
cdefghij  4563
ghi       789
jklmn     1011
```

## Summary

* Use the Visitor pattern to define an operation to be performed on the elements of a hierarchal object structure. 
* Use the Visitor pattern to define the new operation without needing to change the classes of the elements on which it operates.
* When designing your application, you can provision for the future possibility of needing to run custom operations on an element hierarchy, by implementing the Visitor interface in anticipation.
* Usage of the Visitor pattern helps to ensure that your classes conform to the single responsibility principle due to them implementing the custom visitor behavior in a separate class.