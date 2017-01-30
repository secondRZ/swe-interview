# OOP

* In procedural programming, you start with "What does my program do?" and deconstruct your problem from there. With OOP, you start with the question, "What real world objects am I modeling? How would I interact with them? And how do they interact with each other?" Modern teams have PMs that come up with specs. Take those specs to map the classes with CRC cards.

## Classes / Encapsulation

* In C++, each class gets its own header file. Constructors are written like other methods, except with no return type or value, not even void. They are defined in the `public:` space of a class without variable names, just types. 

```cpp
class Rectangle {
  public: 
    Rectangle(int, int);
}

Rectangle::Rectangle(int a, int b) { width_ = a; height_ = b; }
```

* Then `Rectangle my_rect(5, 9);` If your constructor allocates any resources using dynamic memory, your destructor should delete them. Destructor is written like the constructor except with a tilde in front of it.
* Constructors can be overloaded with different types and/or numbers of parameters. The compiler will choose the right one to use. For a default constructor with no parameters, you do not use parenthesis when initializing an object. Just `Rectangle my_rect;` 
* Constructors can also be defined using "member initialization", wherein you put a colon after the parameters, then the member name with parenthesis or curly brackets and the value within them, then the function body, whether blank or not. This should definitely be used if one of the members has a class type that doesn't have a default constructor \(that doesn't have parameters\), because it's the only way to properly build that object.

```cpp
// member initialization
#include <iostream>
using namespace std;

class Circle {
    double radius;
  public:
    Circle(double r) : radius(r) { }
    double area() {return radius*radius*3.14159265;}
};

class Cylinder {
    Circle base;
    double height;
  public:
    Cylinder(double r, double h) : base (r), height(h) {}
    double volume() {return base.area() * height;}
};

int main () {
  Cylinder foo (10,20);

  cout << "foo's volume: " << foo.volume() << '\n'; // foo's volume: 6283.19
  return 0;
}
```

## Inheritance

* The syntax for extending one class from another is `class Rectangle: public Polygon` where Rectangle derives from the Polygon class. 
* You also need to clarify whether you will use the default constructor for the base class or one that accepts arguments.

```cpp
// constructors and derived classes
#include <iostream>
using namespace std;

class Mother {
  public:
    Mother ()
      { cout << "Mother: no parameters\n"; }
    Mother (int a)
      { cout << "Mother: int parameter\n"; }
};

class Daughter : public Mother {
  public:
    Daughter (int a)
      { cout << "Daughter: int parameter\n\n"; }
};

class Son : public Mother {
  public:
    Son (int a) : Mother (a)
      { cout << "Son: int parameter\n\n"; }
};

int main () {
  Daughter kelly(0); // Mother: no parameters -> Daughter: int parameter
  Son bud(0); // Mother: int parameter -> Son: int parameter

  return 0;
}
```

* A class can also inherit from multiple classes by placing a comma between them after the colon. This can come in handy if you want many classes to have the same method, like a print method. Just extend them from that one: `class Triangle: public Polygon, public Output;`

## Diamond Problem

This occurs when a class inherits from two classes, that inherit from the same class. The problem is: if the two parent classes have the same method, which does the lowest child class inherit? The fix is virtual inheritance.

## Polymorphism

* Preceding a method with the `virtual` keyword means that you intend for it to be redefined in classes that are derived from it. Putting the `=0` after the function instead of a body \(not even curly brackets\) makes it a "pure virtual function", which gives derived classes no option to inherit it. It must have its own version. This also makes a class an "abstract class", meaning it is only meant to be inherited as a base, not instatiated. If you try, you'll get an error.
* A pointer of type `BaseClass` can point to the address of type `DerivedClass`. With this, you can use one base type for many types that may have the same method names, but different implementations of that method:

```cpp
// dynamic allocation and polymorphism
#include <iostream>
using namespace std;

class Polygon {
  protected:
    int width, height;
  public:
    Polygon (int a, int b) : width(a), height(b) {}
    virtual int area (void) =0;
    void printarea()
      { cout << this->area() << '\n'; }
};

class Rectangle: public Polygon {
  public:
    Rectangle(int a,int b) : Polygon(a,b) {}
    int area()
      { return width*height; }
};

class Triangle: public Polygon {
  public:
    Triangle(int a,int b) : Polygon(a,b) {}
    int area()
      { return width*height/2; }
};

int main () {
  Polygon * ppoly1 = new Rectangle (4,5);
  Polygon * ppoly2 = new Triangle (4,5);
  ppoly1->printarea(); // 20
  ppoly2->printarea(); // 10
  delete ppoly1;
  delete ppoly2;
  return 0;
}
```

## Abstraction

* Abstraction is the elimination of the irrelevant, and the amplification of the essential. For example, I can teach you how to drive a car by focusing on the steering wheel, the pedals, and the ignition. I leave out details about the transmission, the fuel tank, the battery, etc. I have provided the level of abstraction necessary for you to complete your tasks. Likewise, classes only need expose the state and actions that are necessary, without the end user having to dive into the inner workings of the class.
* Once you've designed and implemented a class, test it. Either manually by calling all methods and watching for the correct actions, or by writing unit tests.
* A static variable of a class is shared by all objects of the class. This can be useful for things like getting how many objects of the class there are.

## Object Oriented Design

1. Gather/create all requirements, user stories, and UI design.
2. Use CRC cards to map each task from each story to a class. Common classes may enclude:
   1. GUI class to be an interface between the system and the user.
   2. A controller object that carries out the use cases in response to interaction with the user. \(The controller has methods that can reach different classes. This allows for a programmer to simply use the controller instead of creating different types of classes to accomplish common tasks.\)
3. Use a sequence diagram to illustrate how each use case is realized by the interaction of objects.
4. Create a class diagram to show how the classes are related to one another.
5. Use a UML diagram to create a detailed picture of each class.

## Design Patterns

* Design patterns are reusable solutions to commonly seen software problems. A few are: Singleton, factory, and adaptor.



