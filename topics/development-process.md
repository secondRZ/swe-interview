# Development Process

#### --------------------------------------------

#### A new task is assigned. Now what?

#### --------------------------------------------

1. If it's a bug, then try to reproduce the bug. You may need to write a unit test for the method.
2. With the specs in hand, go over the OOP design process laid out below. 
3. Build the front end also using TDD.
4. Check your work against all of the tenants of progressive web apps in the web development page. Especially offline considerations.
5. Check your work against all of the tenants of performance in the [web development](/topics/web-development.md) page.
6. Go over the list of code smells.

   1. Code smells for bringing in certain design patterns.

   2. Repeated code means you need to create helper methods/components.

   3. Switch statements or a lot of type checking may mean you need to use inheritance, polymorphism and abstraction. And if you're changing the value of properties/states of data inside of those type checks, you should probably move that state changing to a method in the base class itself, and call it from the place with the type checking.

   4. Once you've properly used OOP, other classes should only interact with the base class, and allow polymorphism to do the work of type checking.

   5. Identical formatting of an object's properties to a string over and over. Just override the ToString\(\) method in that class and use the property itself. See "Point.cs" in the TreehouseDefense project. Ex: `Console.WriteLine("(" + point.X + "," + point.Y + ")")`

   6. Making a copy of an object. Why aren't you just interacting with the actual object? What's wrong with your design?

7. Deployment

   1. Version Number: Major.Minor.Patch

      1. Major: When you make breaking API changes.

      2. Minor: You've added functionality in a backwards compatible manner.

      3. Patch: You've made backwards compatible bug fixes.

      4. Pre-releases: 1.0.1-alpha, 1.0.1-beta

## ------------------------

## Unit Testing

* Purpose: 
  * To ensure that the software is build correctly, and make any bugs that come up easier to find and verify \(by making the test more verbose\).
  * Help you think of range, corner, and edge cases.
  * Discover runtime and logic \(wrong output for an input\) errors.
* Typically, each class is considered a unit. A unit test should test each and every operation that the class provides. Each project should get its own .Tests project.
* Once you have your class models laid out,  start the test writing process with the Red-Green-Refactor method.
  1. Write the tests for each method. There should be errors because the methods don'e exist yet. Use `cmd + .` to create the stub in the classes. Run the tests. Verify that they all fail. There should be range, corner, null, and edge cases.
  2. Now you write the code for the unit that you're testing. Run the tests to make sure they all pass. Sometimes - although you should try to avoid this - a method will depend on another entity \(a class, a remote api, etc\) already being complete. In this case, build a mock/stub class for it \(which is easy if you're using interfaces\).
  3. Refactor the implementation code.

## OOP

* Properties and methods default to `private` access. Even the constructor has to be made `public`.
* A class should control everything about itself. No other classes should be able to change an object's properties without calling a method of that object.
* Extend a base class with a colon. `class Mammal : Animal {`. Base class constructors are called first. The subclass needs a constructor that accepts  parameters that it can pass to the base class, then send it after the subclass' constructer with a colon, the keyword `base`, and the parameters passed. `Mammal(string numLegs, string species) : base(species) {`. The "colon base" thing isn't necessary for constructors without any parameters.
* You can use the this keyword to reference the object itself in class methods.
* Static members are initialized only once, and shared by all objects of the class.
* Syntactic sugar for creating accessor methods for **fields** \(these are called **properties**, typically done whenever the member has public access\):

```java
public MapLocation Location { get; set; }
// shorthand for:

public MapLocation Location
{
  get { return _location; }

  set { _location = value; }
}

// then you can: 

invader.Location = 5;

int num = invader.Location;

// called an "auto-property". and you can completely delete the _location field that it once accessed. 
// Getters and setters are public by default.
// You can also set an initial value like this (instead of putting in the constuctor):

public int Health {get; set;} = 5;
```

* A **computed property** is one where the getter method is written out so that the value is computed every time. This usually happens when a property's value is based on one or more other fields/properties. Instead of updating the property every time the others are changed, you simply compute this one when it is accessed. \(Like an area of a rectangle.\)
* You can use fat arrows with computed properties: `public double Area => SideLength * SideLength;`
* If a class has a property that is simply and array of objects, and that property will have its own behaviors, it may be best to give it its own class since all of the methods of `Array` probably won't be necessary. This practices good **encapsulation**. 
* If a property is not part of a constructor, but you want to set it during instantiation, you can put curly brackets afterward like so:

```java
Level level1 = new Level(invaders) {
    Tower = towers
};
```

* Create a **virtual method** by adding the keyword `virtual` after the access modifier, before the return type \(just like `static`\) inside of the base class: `public virtual void DecreaseHealth(int amount) {}` , and `override` in the derived classes.
* Call the method of a parent class with `base.Method();` This comes in handy when some of the implementation of the virtual method is identical, but not all of it. So you don't have to repeat the identical part.
* Create a **virtual property **the same way. Even if that property is used inside of methods of the base class, when the object is an instance of the child class, it will use the value of the overridden property. You don't have to create a virtual method just for the property. Very good if you h
* You use **polymorphism** and **virtual methods/properties **when different classes with the same parent or interface implementation should have the same method, but a different behavior for that method. 
  * An example is an** **`IAnimal` interface having a method `Move()` since all animals move, but the `Human` class having a different implementation of that method than the `Jaguar` class. \(Bipedal, slower speed, etc.\).
* An object of a certain interface will accept instantiation of any class that uses the interface. So `IDancer myShape = new BreakDancer();` will work, same for methods \(if the method wants a collection of objects that implement the `IDancer` interface, you can put a breakdancer inside\). Now I can have `myDancer.Dance()`, and it will call the right method for it's specific type. The rest of the code base only ever has to deal with the `IDancer` interface, and the virtual methods will take care of the rest. Likewise, if I have a method that simply runs a foreach loop, instead of making it accept a list, make it accept any `IEnumerable`, that way any class that implements that interface can be passed to it.
* Make a class **abstract** by placing `abstract` in front of `class`like this: `abstract class Person {}`
* Making a class abstract means that it isn't meant to be instantiated by the program. It's only an interface. If you want a plain class that has exactly that interface, you'll now need to make a subclass of it.
* Members can also be made `abstract`, and should have `override` in their subclass.
* The purpose of abstract classes is to have a blueprint for subclasses without that blueprint being an actual type that can be instantiated. Usually better to use interfaces instead.
* **Interfaces** are a more clear way of creating such a blueprint. In them, you list all of the properties \(which should be public\) that will be passed on to subclasses. You don't need to put access modifiers, and you don't need to put any accessor methods that aren't public. See the "IInvader.cs" interface as an example. `interface IInvader {}`, then the Base class should have `: InterfaceName`, which does **not **mean it inherits from it. It means that it **implements** that interface. \(See "Invader.cs" for that.\)
* Classes that implement an interface instead of being derived from a base class don't need to pass anything to a base constructor, because they don't have base classes.
* C\# doesn't allow multiple inheritance because of the problems that it can cause. A class implementing multiple interfaces is a workaround to that. It allows a class to inherit from multiple blueprints, without the "diamond problem".
* Make a **static** class by placing `static` in front of `class` like this: `static class Random{}`
* Properties and methods inside of static classes will become **global**. So it's best to make the fields `private`, but the methods public so that they can't be tampered with. \(See "Random.cs"\)
* Here is the process for writing good OOP \(Using interfaces instead of base classes/inheritance\):
* 1. Using a [modeling tool](https://www.genmymodel.com), go through the user stories. **Every **noun and verb that uses two or more nouns should have its own class or interface. If a noun or verb will have different types of itself then it should have its own interface This should help you stick to **SRP**. Adjectives and adverbs are good indicators that there may need to be subclasses.
  2. Stop here and write your tests. See Unit Testing project in the Dev References folder and unit testing section on this page.
  3. Give the interfaces **every method and property/state** that can be called on **all** of its implementing classes, even if the implementation is different for each class, but **not** if only **some **implementing classes will use it and **not** if the property/method could be useful for other types as well. If only some implementing classes will use it, or if it could be used in a more general way by completely unrelated classes, then you should create another interface \(if it's going to be used by certain, related classes, but not all of them\) or a static class \(like a collection of Utils\) with that method/property, and the classes can implement both of the interfaces. 
  4. Write the classes. Think about making a basic version of the class \(See: "BasicInvader"\) that performs each method and sets each property in a way that most implementers of the interface will perform. Then you give those classes an instance of the basic class and simply call the method of the instance for each method implementation, or return the property of the instance for each property. \(See "ResurrectingInvader"\). Copy over all methods and  properties of every interface it implements, and every interface that those interfaces implement. Make them all public, and write the implementations. Then write the extra methods/properties for specific classes:
     1. Type:
        1. Should only ever be accessed within the class itself, not even in the children? **Field**
           1. Different values for each instance? Initialized in the constructor. E.g: `int _score;`
           2. Always the same starting value? Initialized with instantiation. E.g: `int _score = 0;`
        2. Should be accessed outside of the class itself, even if only in subclasses? **Property**
     2. Access:
        1. Should only ever be accessed within the class itself, not even in the children? `private`.
        2. Should only be accessed within the class itself and its children? `protected`.
        3. Should be accessible from anywhere? `public`.
     3. Specialty:
        1. Won't be specific to any one object. More of a general property/method of the class itself? `static` And probably a good candidate to be moved to a static class. \(See Random.cs, Tower.cs, and ShieldedInvader.cs\)
        2. Is a field \(not a property or method\), and should not be changed even within itself? `readonly`
        3. **Must **be overridden in subclasses? \(As in, there is no generic way to do this method or generic value this property should be. They'll all be different, so they should all declare their own version\) `abstract` The subclasses should have `override` in its place.
        4. **Can** be overridden in subclasses? `virtual`
        5. Is not specific to one instance of the class, rather it is useful for the class as a whole? `static`
     4. Data Type:
        1. Numeric:
        2. Collection \(by most common operation\):
           1. Most often looking up by index. You care about the order.: `List` 
           2. Shouldn't have any kind of direct element access \(Square bracket notation, Remove\(\), etc\)? `IEnumerable`
           3. Most often looking up by value. Don't care about order. No duplicates in collection. `HashSet`.
           4. Same as above but looking up by key. `Dictionary`.
        3. Generic:
     5. Accessor methods \(for properties\):
        1. Property value is based on the computation of one or more other field/property values? **Computed property**
        2. Initial value is magic **and** not member is **not** abstract? Give it a value after the parenthesis. `public int Size { get; private set; } = 1;`
        3. `get;`
           1. Almost always public. Leave it alone unless its a computed property.
        4. `set;`
           1. Value should not be changed, even within itself \(basically a const, but a **property** because we want it accessible elsewhere.\): Leave `set;` out of the definition. No setter. Making it a readonly property everywhere.
           2. Value should only be accessed within the class itself, not even subclasses. `private set;`
           3. Value should be accessed only by itself and its subclasses \(this should be the most accessible level for the setter, never just public\) `protected set;` 
        5. No `set;` **and** initial value is magic? Use a computed property. `public int Size => 1;`
  5. Construct the interfaces:
     1. It should include each public method and property of the abstract base class, but it doesn't have to include them all in itself. It can be composed of multiple interfaces.
     2. 1. Does **every** subclass that will implement this interface use this property/method **and** is this property/method specific to this kind of class, not really for other types of objects? Put it in this interface.
        2. Else put it in its own interface that the subclass can inherit from on its own.
     3. Once its constructed, 

## Backend Development

* Data Processing
  * Encoding is a map. A rosetta stone for the computer to know how to turn the characters into bytes.



