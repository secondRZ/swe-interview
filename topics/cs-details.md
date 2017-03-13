# C\# Details

## Program Structure

* Namespaces should be `CompanyName.ActualNamespace` and can be accessed from different files, so you can still give each class its own file.
* The `var` keyword only works when the assignment comes at the same time. `var some_int;` wouldn't work.
* Generate a random number: `System.Random rand = new System.Random();` Then `rand.Next();` or `rand.NextDouble();`
* Just like C++, you can use `using SomeNameSpace;` at the top to avoid typing it every time, but it's better not to.
* You can make a variable/property read only with the `readonly` keyword before its type: `public readonly int height;`.
* Fat arrow methods work in C\#. Different syntax though. `public void Move() => _location++;`. You know it's a method \(not a property\) because of the parenthesis. You know whether it returns something because of the stated return type.
* Accept input from a user:

```java
static void Main() {
    System.Console.Write("Enter how many minutes you exercised: ");    
    string entry = System.Console.ReadLine();
}
```

## Data Types

* **System.Object:** All classes inherit from System.Object, and they all have these methods:
  * `object.Equals(Other)`: Returns true if `object` is equal to `Other`; false otherwise. By default it just checks if the objecs are actually the same object. Should be overridden if you want to check properties agains each other.
  * `System.Object.Equals(object, other)`: Returns true if `object` and `other` are equal; false otherwise.
* * `object.GetType()` will return the type of `object`. Conversely, `object is SomeType` will return a true if `object` is of that type; false otherwise.
  * `object.MemberwiseClone()`: Creates a shallow copy of `object`.
  * `object.ToString()`: Returns a string that represents `object`. Should be overridden if you want actual functionality. If you override a base class, the subclass will of course get the ToString method as well.
* **bool**
* **string**: Always between two quotes.
  * `ToLower("C")` and `ToUpper("c")`
* **int**:
  * `int.Parse("55");` returns the converted `string` **55** into an `int`.

## Exceptions

* Send a message: `throw new System.Exception("Some message")`. And in `catch(System.Exception ex)` you can log the `ex.Message`
* It is wise to create your own classes of exceptions for your app, to make catching them more specific. The base exception of your app should inherit from System.Exception. And others from your app should inherit from that one. Make sure they have at least 2 constructors. One that accepts a message, and one that doesn't.
* Parents also catch children exceptions.
* When you use multiple catch statements, the most specific exceptions \(that is, those lowest in the inheritance tree, the child with the least children\) should go first.

## Collections

* **Array**: `string[] people = new string[3];` or `string[] people = {"Brandon", "Jacob", "Bob"};`
  * `people.Length`: Return the length of the array.
  * Pass array literals into a method with `SomeMethod(new []{"Brandon", "Jacob", "Bob"});`

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
* You use **polymorphism** and **virtual methods/properties **when different classes with the same parent should have the same method, but a different behavior for that method** and **there is some actual use of the parent class. 
  * An example is an** **`Animal` class having a method `Move()` since all animals move, but the `Human` class having a different implementation of that method than the `Jaguar` class. \(Bipedal, slower speed, etc.\).
* An object of a certain type will accept instantiation of children of its class as well. So `Polygon myShape = new Square();` will work, same if you want an array of polygons and put a square inside. When you mix this with virtual methods, you get the real magic of polymorphism. Now I can have `myShape.GetArea()`, and it will call the right method for it's specific type, but it will also fit in places that are expect a `Polygon` specifically. The rest of the code base only ever has to deal with the `Polygon` class, and the virtual methods will take care of the rest.
* Make a class **abstract** by placing `abstract` in front of `class`like this: `abstract class Person {}`
* Making a class abstract means that it isn't meant to be instantiated by the program. It's only an interface. If you want a plain class that has exactly that interface, you'll now need to make a subclass of it.
* Members can also be made `abstract`, and should have `override` in their subclass.
* The purpose of abstract classes is to have a blueprint for subclasses without that blueprint being an actual type that can be instantiated. Usually better to use interfaces instead.
* **Interfaces** are a more clear way of creating such a blueprint. In them, you list all of the properties \(which should be public\) that will be passed on to subclasses. You don't need to put access modifiers, and you don't need to put any accessor methods that aren't public. See the "IInvader.cs" interface as an example. `interface IInvader {}`, then the Base class should have `: InterfaceName`, which does **not **mean it inherits from it. It means that it **implements** that interface. \(See "Invader.cs" for that.\)
* Classes that implement an interface instead of being derived from a base class don't need to pass anything to a base constructor, because they don't have base classes.
* C\# doesn't allow multiple inheritance because of the problems that it can cause. A class implementing multiple interfaces is a workaround to that. It allows a class to inherit from multiple blueprints, without the "diamond problem".
* Here is the process for writing good OOP:
* 1. Using Pastebin, go through the user stories. **Every **noun and verb should have its own class. If a class will have subclasses then it should have its own abstract class \(and base class if you need a class that is exactly the same as the abstract\). This should help you stick to **SRP**. Abstract classes should end with "Base" \(PersonBase\), and if it has a subclass with no changes to the interface they should start with "Basic" \(BasicPerson\). Subclasses will inherit from the abstract class. 
  2. Give the base classes **every method and property/state** that can be called on **all** of the subclasses, even if the implementation is different for each subclass, but **not** if only **some **subclasses will use it. If only some subclasses will use it, then you should create another base class with that method/property, and the subclasses can implement both of the interfaces for those base classes. This keeps you close to **L** in SOLID \(Liskov substitution \(a subclass should be substitutable with its base class\)\), and the **I** \(classes shouldn't be forced to implement **interfaces** they don't use\). Sometimes when you do this, because of polymorphism, you'll have implementations of a base class that calls a method or accesses a property that a child doesn't have \(which breaks the **L **principle\), so there should be a third base class that manages differences between the others. \(Ex: All objects of the `Shapes` class have an `area`, but not a `volume`. So you create the `ThreeDShapes` class and give it the `volume` property. Some places in the code want to calculate `totalSpace`, which is different if you have a volume vs not. So you make a third class called `SpaceManager` which has a method to calculate totalSpace. Now shapes can inherit from any combination of `Shapes` and `ThreeDShapes`, and all should inherit from `SpaceManager`.\) The implementation of each method should be what you imagine a generic instance of this class to look like.
     1. Type:
        1. Should only ever be accessed within the class itself, not even in the children? **Field**
           1. Different values for each instance? Initialized in the constructor. E.g: `int _score;`
           2. Always the same starting value? Initialized with instantiation. E.g: `int _score = 0;`
           3. Should be accessed outside of the class itself, even if only in subclasses? **Property**
     2. Access:
        1. Should only ever be accessed within the class itself, not even in the children? `private`.
        2. Should only be accessed within the class itself and its children? `protected`.
        3. Should be accessible from anywhere? `public`.
     3. Specialty:
        1. Is a field \(not a property or method\), and should not be changed even within itself? `readonly`
        2. **Must **be overridden in subclasses? \(As in, there is no generic way to do this method or generic value this property should be. They'll all be different, so they should all declare their own version\) `abstract` The subclasses should have `override` in its place.
        3. **Can** be overridden in subclasses? `virtual`
        4. Is not specific to one instance of the class, rather it is useful for the class as a whole? `static`
     4. Accessor methods \(for properties\):
        1. Property value is based on the computation of one or more other field/property values? **Computed property**
        2. Initial value is magic **and** not member is **not** abstract? Give it a value after the parenthesis. `public int Size { get; private set; } = 1;`
        3. `get;`
           1. Almost always public. Leave it alone unless its a computed property.
        4. `set;`
           1. Value should not be changed, even within itself \(basically a const, but a **property** because we want it accessible elsewhere.\): Leave `set;` out of the definition. No setter. Making it a readonly property everywhere.
           2. Value should only be accessed within the class itself, not even subclasses. `private set;`
           3. Value should be accessed only by itself and its subclasses \(this should be the most accessible level for the setter, never just public\) `protected set;` 
        5. No `set;` **and** initial value is magic? Use a computed property. `public int Size => 1;`
  3. Construct the interfaces:
     1. The name of each should be the name of its base abstract class with an "I" in front of it. 
     2. It should include each public method and property of the abstract base class, but it doesn't have to include them all in itself. It can be composed of multiple interfaces.
        1. Does **every** subclass that will implement this interface use this property/method **and** is this property/method specific to this kind of class, not really for other types of objects? Put it in this interface.
        2. Else put it in its own interface that the subclass can inherit from on its own.
     3. Once its constructed, 
  4. Construct the subclasses:
     1. The subclasses must have at least one constructor for every constructor the base class has that takes any parameters. At the minimum, passing on the necessary arguments. `public Child(Ethnicity eth) : base(eth) {}`



