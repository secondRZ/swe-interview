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
  * `string.Join(" ");` Join items in a list by the delimiter into a string.
  * `string.IsNullOrWhitespace(variable)`: Checks for and empty value.
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
* A reason to make a collection **IEnumerable** instead of a List, is to essentially make it readonly, since it won't have any add or access methods. But **IReadOnlyList** is a more specific way to do that.
* **List**: `List<string> myList = new List<string>(optionalInitialCapactyInt) {"Optional", "initialized", "values"};`
  * `myList.Add("Item");`: Adds an item to the end of the list.
  * `myList.Count;`: Returns the length of the list.
  * `myList.ToArray();`: Converts a list to an array.
  * `myList.Insert(2, "Brandon");`: Insert at index 2.
  * `myList.RemoveAt(2);`: Remove at index 2.
  * `myList.Remove("Brandon")`: Returns true if it could remove the item. The first one it finds.
  * `myList.Contains("Brandon")`: Returns true if object is in the list.
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

* **HashSet**: Constant lookup time by value. Unordered. No duplicates added. `HashSet<Student> students = new HashSet<Student>(optionalListToInitializeWith);`.
  * When using objects, be careful about duplicates. By default, each object has it's own unique hashcode. You need to override `GetHashCode` to make the `HashSet` compare by value. If it came from a database, it probably has a unique id that you can simply return the `GetHashCode` of, otherwise, you can probably return the sum of `.GetHashCode()` of the properties that you care to compare for equality. If you override `GetHashCode`, you must also override `Equals`. Do this by just checking if the actual values of those properties are the same. \(Eg: `Name = obj.Name`\)
* **Dictionary**: Holds key/value pairs. `Dictionary<char, string> people = new Dictionary<char, string>();`.

  * Loop over a dictionary with `foreach(KeyValuePair<char, string> pair in _dict) { pair.Value and pair.Key}`.

  * Add with square brackets or with `.Add()`, but if a key already exists while using `.Add()` then an exception is thrown.

  * `.Keys` and `.Values` are properties of each object that return collections for each respectively.

  * `.ContainsKey()` and `.ContainsValue()` search for both respectively, though if you're only searching for values, you may be better off with a hashset.

## LINQ

* Linq is a way to query with your code. It looks a lot like JS filter. You can use it on anything that implements IEnumerable&lt;T&gt;
* Start by `using System.Linq;`
* See the LINQ project for syntax.
  * **Where**: Like JS filter.
  * **FirstOrDefault**: Returns just one element of a filter or default \(null, 0, etc\).
  * **OrderBy**: Better than Sort\(\) because you don't have to create a sortable class.
  * **ThenBy**
  * **All**: Like JS every;
  * **Any**: Like JS any.
  * **Select**: Create an anonymous object, or extract one property into a list like \_.pluck\(\);
  * **SelectMany**: Flattens queries that return a list of lists.
  * **Take**: Select the top n items. Usually after an OrderBy.
  * **Skip**: Skip some before taking some. Creates pages.
  * **Join**: Good for if your Where clause has multiple checks for the same property \(color == "Red" \|\| color == "Blue"\).
  * **GroupBy**: Group elements by a property.
  * **Sum**, **Average**, **Min**, and **Max**: Calculate for some property for items in the list.
  * **Except**: Returns a list of items from another list that are not present in the first.
  * **Range**: Add n numbers to a list from a certain number.
  * **Intersect**: Returns a list that are present in both.
