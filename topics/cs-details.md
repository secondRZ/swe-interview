# C\# Details

## Program Structure

* Namespaces should be `CompanyName.ActualNamespace` and can be accessed from different files, so you can still give each class its own file.
* The `var` keyword only works when the assignment comes at the same time. `var some_int;` wouldn't work.
* Generate a random number: `System.Random rand = new System.Random();` Then `rand.Next();` or `rand.NextDouble();`
* Just like C++, you can use `using SomeNameSpace;` at the top to avoid typing it every time, but it's better not to.
* You can make a variable/property read only with the `readonly` keyword before its type: `public readonly int height;`.
* Fat arrow methods work in C\#. Different syntax though. `public void Move() => _location++;`. You know it's a method \(not a property\) because of the parenthesis. You know whether it returns something because of the stated return type.
* The **System.IO** namespace has the classes and methods for reading and writing data.
* Accept input from a user with **Console.ReadLine\(\)**:

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
  * For a two dimensional array: `int[,] arr = new int[100, 10];` You set values like so:

```java
namespace Treehouse.CodeChallenges
{
    public static class MathHelpers
    {
        public static int[,] BuildMultiplicationTable(int maxFactor)
        {
            int[,] arr = new int[maxFactor + 1, maxFactor + 1];
            for (int i = 0; i < arr.GetLength(0); i++)
                for (int j = 0; j < arr.GetLength(1); j++)
                    arr[i, j] = i * j;
            return arr;
        }
    }
}
```

* Use generic collections with `using System.Collections.Generic;`
* Your own classes can have `foreach` if they implement **IEnumerable**. You get that plust `Add`, `Clear`, `Contains`, and `Remove` methods, and the `Count` property if they implement   **ICollection**. Implementing **IList** will get you all of that plus bracket notation, `IndexOf`, `Insert`, `Remove`, and `RemoveAt`.
* **List**: `List<string> myList = new List<string>(optionalInitialCapactyInt) {"Optional", "initialized", "values"};`
  * `myList.Add("Item");`: Adds an item to the end of the list.
  * `myList.Count;`: Returns the length of the list.
  * `myList.ToArray();`: Converts a list to an array.
  * `myList.Insert(2, "Brandon");`: Insert at index 2.
  * `myList.RemoveAt(2);`: Remove at index 2.
  * `myList.Remove("Brandon")`: Returns true if it could remove the item. The first one it finds.
  * `myList.IndexOf("Brandon");`: Returns the first index of the search argument, or -1 if not there.
  * `myList.Sort();`: Sorts the list. To sort objects, make sure the class implements the `IComparable<ClassName>` interface.

```java
public int CompareTo(Student that)
{
    int result = Name.CompareTo(that.Name);
    if (result == 0)
    {
        result = GradeLevel.CompareTo(that.GradeLevel);
    }

    return result;
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
* Create a **virtual property **the same way. Even if that property is used inside of methods of the base class, when the object is an instance of the child class, it will use the value of the overridden property. You don't have to create a virtual method just for the property. Very good if you h
* You use **polymorphism** and **virtual methods/properties **when different classes with the same parent or interface implementation should have the same method, but a different behavior for that method. 
  * An example is an** **`IAnimal` interface having a method `Move()` since all animals move, but the `Human` class having a different implementation of that method than the `Jaguar` class. \(Bipedal, slower speed, etc.\).
* An object of a certain interface will accept instantiation of any class that uses the interface. So `IDancer myShape = new BreakDancer();` will work, same if you want an array of objects that implement the `IDancer` interface and put a breakdancer inside. Now I can have `myDancer.Dance()`, and it will call the right method for it's specific type. The rest of the code base only ever has to deal with the `IDancer` interface, and the virtual methods will take care of the rest.
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
* 1. Using Pastebin, go through the user stories. **Every **noun and verb should have its own class or interface. If a noun or verb will have different types of itself then it should have its own interface This should help you stick to **SRP**. 
  2. Give the interfaces **every method and property/state** that can be called on **all** of its implementing classes, even if the implementation is different for each class, but **not** if only **some **implementing classes will use it and **not** if the property/method could be useful for other types as well. If only some implementing classes will use it, or if it could be used in a more general way by completely unrelated classes, then you should create another interface \(if it's going to be used by certain, related classes, but not all of them\) or a static class \(like a collection of Utils\) with that method/property, and the classes can implement both of the interfaces. 
  3. Write the classes. Think about making a basic version of the class \(See: "BasicInvader"\) that performs each method and sets each property in a way that most implementers of the interface will perform. Then you give those classes an instance of the basic class and simply call the method of the instance for each method implementation, or return the property of the instance for each property. \(See "ResurrectingInvader"\). Copy over all methods and  properties of every interface it implements, and every interface that those interfaces implement. Make them all public, and write the implementations. Then write the extra methods/properties for specific classes:
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
        1. Won't be specific to any one object. More of a general property/method of the class itself? `static` And probably a good candidate to be moved to a static class. \(See Random.cs, Tower.cs, and ShieldedInvader.cs\)
        2. Is a field \(not a property or method\), and should not be changed even within itself? `readonly`
        3. **Must **be overridden in subclasses? \(As in, there is no generic way to do this method or generic value this property should be. They'll all be different, so they should all declare their own version\) `abstract` The subclasses should have `override` in its place.
        4. **Can** be overridden in subclasses? `virtual`
        5. Is not specific to one instance of the class, rather it is useful for the class as a whole? `static`
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
  4. Construct the interfaces:
     1. It should include each public method and property of the abstract base class, but it doesn't have to include them all in itself. It can be composed of multiple interfaces.
     2. 1. Does **every** subclass that will implement this interface use this property/method **and** is this property/method specific to this kind of class, not really for other types of objects? Put it in this interface.
        2. Else put it in its own interface that the subclass can inherit from on its own.
     3. Once its constructed, 

## Backend Development

* Data Processing
  * Encoding is a map. A rosetta stone for the computer to know how to turn the characters into bytes.



