# C\# Details

## Program Structure

* Namespaces should be `CompanyName.ActualNamespace` and can be accessed from different files, so you can still give each class its own file.
* The `var` keyword only works when the assignment comes at the same time. `var some_int;` wouldn't work.
* Just like C++, you can use `using SomeNameSpace;` at the top to avoid typing it every time, but it's better not to.
* You can make a variable/property read only with the `readonly` keyword before its type: `public readonly int height;`.
* Accept input from a user:

```java
static void Main() {
    System.Console.Write("Enter how many minutes you exercised: ");    
    string entry = System.Console.ReadLine();
}
```

## OOP

* Properties and methods default to `private` access. Even the constructor has to be made `public`.
* Extend a base class with a colon. `class Mammal : Animal {`. Base class constructors are called first. The subclass needs a constructor that accepts  parameters that it can pass to the base class, then send it after the subclass' constructer with a colon, the keyword `base`, and the parameters passed. `Mammal(string numLegs, string species) : base(species) {`. The "colon base" thing isn't necessary for constructors without any parameters.
* You can use the this keyword to reference the object itself in class methods.
* Syntactic sugar for creating accessor methods \(these are called properties\):

```java
public MapLocation Location
{
  get 
  {
    return _location;
  }

  private set
  {
    _location = value;
  }
}

invader.Location = 5;

int num = invader.Location;
```

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


