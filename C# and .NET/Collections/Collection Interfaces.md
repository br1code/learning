# Collection Interfaces

## Simple Explanation

Collection interfaces in .NET are a set of predefined interfaces that define common operations that can be performed on collections of objects. The most commonly used collection interfaces are IEnumerable, ICollection, IList.

## Deep Explanation

The IEnumerable interface is the most basic collection interface and provides the ability to iterate over a collection of objects using a foreach loop. It contains a single method, GetEnumerator(), which returns an IEnumerator that allows you to iterate over the collection. **Use IEnumerable when you want to iterate over a collection of items without modifying it.** 

The ICollection interface inherits from IEnumerable and adds the ability to modify a collection of objects. It includes methods such as Add(), Remove(), and Contains() that allow you to add, remove, and check for the presence of elements in a collection. **Use ICollection when you want to add or remove items from a collection.** Note: IReadOnlyCollection is similar to ICollection, but it only provides read-only access to a collection, meaning you can't add or remove items from the collection. If you just need to iterate over a collection without modifying it, you can use either IEnumerable or IReadOnlyCollection. However, if you need to expose the count of the number of items in the collection, then you should use IReadOnlyCollection.

The IList interface inherits from both IEnumerable and ICollection, and adds the ability to index into a collection of objects using an integer index. It includes methods such as Insert(), RemoveAt(), and IndexOf() that allow you to add, remove, and locate elements in a collection. **Use IList when you need to access items in a collection by their index.**

## Examples

```C#
// IEnumerable example
IEnumerable<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
foreach (int num in numbers)
{
    Console.WriteLine(num);
}

// ICollection example
ICollection<string> names = new List<string> { "John", "Jane", "Bob" };
names.Add("Alice");
names.Remove("Bob");
if (names.Contains("John"))
{
    Console.WriteLine("John is in the list");
}

// IList example
IList<int> myList = new List<int> { 1, 2, 3 };
myList.Insert(1, 5);
myList.RemoveAt(2);
int index = myList.IndexOf(5);
```