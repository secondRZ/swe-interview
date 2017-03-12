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
* You use **polymorphism** and **virtual methods **when different classes with the same parent should have the same method, but a different behavior for that method** and **there is some actual use of the parent class. 
  * An example is an** **`Animal` class having a method `Move()` since all animals move, but the `Human` class having a different implementation of that method than the `Jaguar` class. \(Bipedal, slower speed, etc.\).
* An object of a certain type will accept instantiation of children of its class as well. So `Polygon myShape = new Square();` will work, same if you want an array of polygons and put a square inside. When you mix this with virtual methods, you get the real magic of polymorphism. Now I can have `myShape.GetArea()`, and it will call the right method for it's specific type, but it will also fit in places that are expect a `Polygon` specifically. The rest of the code base only ever has to deal with the `Polygon` class, and the virtual methods will take care of the rest.
* Here is the process for making your code polymorphic:
  * Give the base class should have **every method and property/state** that can be called on the subclasses, even if the implementation is different for each subclass. The implementation of each method should be what you imagine a generic instance of this class to look like.
    * Type:
      * Should only ever be accessed within the class itself, not even in the children? **Field**
        * Different values for each instance? Initialized in the constructor. E.g: `int _score;`
        * Always the same starting value? Initialized with instantiation. E.g: `int _score = 0;`
      * Should be accessed outside of the class itself, even if only in subclasses? **Property**
  * * Access:
      * Should only ever be accessed within the class itself, not even in the children? `private`.
      * Should only be accessed within the class itself and its children? `protected`.
      * Should be accessible from anywhere? `public`.
    * Specialty:
      * Is a field \(not a property or method\), and should not be changed even within itself? `readonly`
      * Will be overridden in subclasses? `virtual`
      * Is not specific to one instance of the class, rather it is useful for the class as a whole? `static`
    * Accessor methods \(for properties\):
      * Property value is based on the computation of one or more other field/property values? **Computed property**
      * Initial value is magic? Give it a value after the parenthesis. `public int size { get; private set; } = 1;`
      * `get;`
        * Almost always public. Leave it alone unless its a computed property.
      * `set;`
        * Value should not be changed, even within itself \(basically a const, but a **property** because we want it accessible elsewhere.\): Leave `set;` out of the definition. No setter. Making it a readonly property everywhere.
        * Value should only be changed within the class itself, not even subclasses. `private set;`
        * Value should be changed only by itself and its subclasses \(this should be the most accessible level for the setter, never just public\) `protected set;` 

## Data Types

* **Type checking:** `variable.GetType()` will return the type of the variable. `x is SomeType` will return a boolean if `x` is of that type.
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
* 


