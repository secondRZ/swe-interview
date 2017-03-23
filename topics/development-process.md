# Development Process

#### --------------------------------------------

#### A new task is assigned. Now what?

#### --------------------------------------------

1. With the specs in hand, go over the OOP design process laid out in the [C\# Details](/topics/cs-details.md) page.
2. Check for edge, null, and range cases for all of the data points you're dealing with.
3. Check your work against all of the tenants of progressive web apps in the web development page. Especially offline considerations.
4. Check your work against all of the tenants of performance in the [web development](/topics/web-development.md) page.
5. Go over the list of code smells.  
   1. Code smells for bringing in certain design patterns.

   1. Repeated code means you need to create helper methods/components.

   2. Switch statements or a lot of type checking may mean you need to use inheritance, polymorphism and abstraction. And if you're changing the value of properties/states of data inside of those type checks, you should probably move that state changing to a method in the base class itself, and call it from the place with the type checking.

   3. Once you've properly used OOP, other classes should only interact with the base class, and allow polymorphism to do the work of type checking.

   4. Identical formatting of an object's properties to a string over and over. Just override the ToString\(\) method in that class and use the property itself. See "Point.cs" in the TreehouseDefense project. Ex: `Console.WriteLine("(" + point.X + "," + point.Y + ")")`

   5. Making a copy of an object. Why aren't you just interacting with the actual object? What's wrong with your design?

6. Deployment

   1. Version Number: Major.Minor.Patch

      1. Major: When you make breaking API changes.

      2. Minor: You've added functionality in a backwards compatible manner.

      3. Patch: You've made backwards compatible bug fixes.

      4. Pre-releases: 1.0.1-alpha, 1.0.1-beta



