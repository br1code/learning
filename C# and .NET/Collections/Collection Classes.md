# Common Collection Classes

## List<T>

### Simple Explanation

List is a collection of items that you can add, remove or modify easily.

### Deep Explanation

List<T> is a generic class in .NET that is similar to an array, but with the added benefit of being resizable. It allows for easy addition, removal, and modification of elements. List is implemented as a dynamic array, meaning it is stored in a contiguous block of memory, and if more space is needed, it can allocate a new larger block and copy the elements over. List<T> provides several methods for working with the collection, including Add, Remove, Insert, and Count.

### Examples

```C#
List<int> numbers = new List<int>();
numbers.Add(1);
numbers.Add(2);
numbers.Add(3);
Console.WriteLine(numbers.Count); // Output: 3
numbers.Remove(2);
Console.WriteLine(numbers.Count); // Output: 2
```

## Dictionary<TKey, TValue>

### Simple Explanation

Dictionary is a collection of key-value pairs where you can access values quickly based on their keys.

### Deep Explanation

Dictionary<TKey, TValue> is a generic class in .NET that represents a collection of key-value pairs. Each key in the dictionary must be unique. It provides fast access to values based on their keys. Dictionary is implemented using a hash table, which provides fast lookup times, even for large collections. Dictionary<TKey, TValue> provides several methods for working with the collection, including Add, Remove, ContainsKey, and Count.

### Examples

```C#
Dictionary<string, int> ages = new Dictionary<string, int>();
ages.Add("John", 30);
ages.Add("Mary", 25);
ages.Add("Bob", 40);
Console.WriteLine(ages["Mary"]); // Output: 25
ages.Remove("Bob");
Console.WriteLine(ages.Count); // Output: 2
```

## HashSet<T>

### Simple Explanation

HashSet is a collection of unique elements that doesn't allow duplicates.

### Deep Explanation

HashSet<T> is a generic class in .NET that represents a collection of unique elements. It does not allow duplicates and is implemented using a hash table, providing fast lookup times. HashSet<T> provides several methods for working with the collection, including Add, Remove, Contains, and Count.


### Examples

```C#
HashSet<string> names = new HashSet<string>();
names.Add("John");
names.Add("Mary");
names.Add("Bob");
Console.WriteLine(names.Count); // Output: 3
names.Remove("Bob");
Console.WriteLine(names.Contains("Mary")); // Output: True
```

## Stack<T>

### Simple Explanation

Stack is a collection that follows the Last-In, First-Out (LIFO) rule. You can push elements onto the stack and pop elements off the top of the stack.

### Deep Explanation

Stack<T> is a generic class in .NET that represents a last-in, first-out (LIFO) collection. It provides methods to push elements onto the stack and pop elements off the top of the stack. Stack<T> is implemented using an array, and it grows dynamically as more elements are added.  It is useful for keeping track of the order of operations, such as in undo-redo scenarios. The main methods of a Stack<T> are Push, Pop, and Peek.

### Examples

```C#
Stack<int> myStack = new Stack<int>();
myStack.Push(1);
myStack.Push(2);
myStack.Push(3);
Console.WriteLine(myStack.Pop()); // prints 3
Console.WriteLine(myStack.Peek()); // prints 2
```

## Queue<T>

### Basic Explanation

A Queue is a collection that represents a first-in, first-out (FIFO) sequence of elements. It can be visualized as a line of people waiting for a service, where the first person to join the line is the first person to be served. In a queue, elements are added at the end and removed from the front.

### Deep Explanation

In C# and .NET, the Queue class is used to implement a queue data structure. It's a generic class, which means it can hold elements of any data type. The Queue class provides several methods to manipulate the queue, such as Enqueue(), Dequeue(), Peek(), and Count. Here's a brief explanation of each:

- Enqueue(): adds an element to the end of the queue.

- Dequeue(): removes and returns the element at the beginning of the queue. Throws an exception if the queue is empty.

- Peek(): returns the element at the beginning of the queue without removing it. Throws an exception if the queue is empty.

- Count: gets the number of elements currently in the queue.

Like other collection classes in C# and .NET, the Queue class implements the IEnumerable and ICollection interfaces, which means that it can be used with LINQ queries and can be easily converted to an array, list, or other collection types.

### Examples

```C#
Queue<int> myQueue = new Queue<int>();
myQueue.Enqueue(1);
myQueue.Enqueue(2);
myQueue.Enqueue(3);
Console.WriteLine(myQueue.Dequeue()); // prints 1
Console.WriteLine(myQueue.Peek()); // prints 2
```