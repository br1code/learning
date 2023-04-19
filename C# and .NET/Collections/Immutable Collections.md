# Immutable Collections

## Simple Explanation

Immutable Collections in C# and .NET are data structures that cannot be modified once they are created. Instead of modifying an existing collection, operations on immutable collections return a new collection with the desired changes. This makes immutable collections thread-safe and can improve performance in some scenarios.

## Deep Explanation

Immutable Collections are part of the System.Collections.Immutable namespace in C# and .NET. They provide a way to create collections that cannot be modified after they are created. This is achieved by returning a new collection whenever an operation is performed that would modify the collection. The original collection remains unchanged, and a new collection is created with the desired changes.

Immutable Collections are thread-safe because multiple threads can read from an immutable collection at the same time without the need for locking or synchronization. Since the collection cannot be modified, there is no risk of data corruption due to concurrent modification.

Immutable Collections can also improve performance in certain scenarios. Since modifying a collection requires creating a new collection, immutable collections can be more efficient when many operations are performed on a collection. For example, if you need to perform many modifications to a list, creating a new list for each modification can be faster than modifying the existing list, especially if the list is large.

Immutable Collections are available for many common collection types, including lists, dictionaries, sets, and stacks. You can create immutable collections using the ImmutableArray, ImmutableList, ImmutableDictionary, ImmutableHashSet, and ImmutableStack classes, among others.

## Examples

```C#
// Creating an immutable array
ImmutableArray<int> array1 = ImmutableArray.Create(1, 2, 3);

// Accessing an item in an immutable array
int item1 = array1[0]; // item1 = 1

// Updating an item in an immutable array
ImmutableArray<int> array2 = array1.SetItem(1, 4); // array2 contains [1, 4, 3]

// Removing an item from an immutable array
ImmutableArray<int> array3 = array2.RemoveAt(2); // array3 contains [1, 4]
```

```C#
// Creating an immutable list
ImmutableList<int> list1 = ImmutableList.Create(1, 2, 3);

// Adding an item to an immutable list
ImmutableList<int> list2 = list1.Add(4);

// Removing an item from an immutable list
ImmutableList<int> list3 = list2.Remove(2);
```

```C#
// Creating an immutable dictionary
ImmutableDictionary<string, int> dict1 = ImmutableDictionary.Create<string, int>()
    .Add("one", 1)
    .Add("two", 2)
    .Add("three", 3);

// Updating an item in an immutable dictionary
ImmutableDictionary<string, int> dict2 = dict1.SetItem("two", 4);

// Removing an item from an immutable dictionary
ImmutableDictionary<string, int> dict3 = dict2.Remove("three");
```

```C#
// Creating an immutable hash set
ImmutableHashSet<string> set1 = ImmutableHashSet.Create("foo", "bar", "baz");

// Adding an item to an immutable hash set
ImmutableHashSet<string> set2 = set1.Add("qux");

// Removing an item from an immutable hash set
ImmutableHashSet<string> set3 = set2.Remove("bar");
```

```C#
// Creating an immutable stack
ImmutableStack<int> stack1 = ImmutableStack.CreateRange(new[] { 1, 2, 3 });

// Pushing an item onto an immutable stack
ImmutableStack<int> stack2 = stack1.Push(4);

// Popping an item off an immutable stack
int item = stack2.Peek(); // item = 4
ImmutableStack<int> stack3 = stack2.Pop(); // stack3 contains [1, 2, 3]
```