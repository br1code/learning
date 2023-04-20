# Memento

## Simple Explanation

The Memento design pattern is a behavioral design pattern that captures and externalizes an object's internal state without violating encapsulation, allowing the object to be restored to this state later. It's particularly useful when you need to implement undo/redo functionality or save the state of an object for later use.

## Deep Explanation

The Memento pattern involes three main components:

1. Originator - the object whose state needs to be saved and restored. It creates mementos to capture its state and can restore its state from a memento.

2. Memento - an object that stores the internal state of the originator. It contains the necessary data to restore the originator to a previous state.

3. Caretaker - an object that manages mementos, keeping track of when and why they were created. The Caretaker does not modify mementos or have direct access to their contents.

By implementing the Memento pattern, you can provide a way to save and restore the state of an object wihout exposing its implementation details or violating encapsulation.

## Examples

Image we need to implement undo funcionality for a text editor.

1. Create the Memento class:

```C#
public class TextMemento
{
    public string Text { get; }

    public TextMemento(string text)
    {
        Text = text;
    }
}
```

2. Implement the Originator class:

```C#
public class TextEditor
{
    public string Text { get; set; }

    public TextMemento SaveToMemento()
    {
        return new TextMemento(Text);
    }

    public void RestoreFromMemento(TextMemento memento)
    {
        Text = memento.Text;
    }
}
```

3. Implement the Caretaker class:

```C#
public class UndoManager
{
    private readonly Stack<TextMemento> _mementos = new Stack<TextMemento>();

    public void Save(TextEditor editor)
    {
        _mementos.Push(editor.SaveToMemento());
    }

    public void Undo(TextEditor editor)
    {
        if (_mementos.Count > 0)
        {
            TextMemento memento = _mementos.Pop();
            editor.RestoreFromMemento(memento);
        }
    }
}
```

4. Use the Memento and Caretaker objects:

```C#
class Program
{
    static void Main(string[] args)
    {
        var editor = new TextEditor();
        var undoManager = new UndoManager();

        editor.Text = "Hello friend!";
        undoManager.Save(editor);

        editor.Text = "Hello memento!";
        undoManager.Save(editor);

        Console.WriteLine(editor.Text); // Output: "Hello memento!"

        undoManager.Undo(editor);
        Console.WriteLine(editor.Text); // Output: "Hello friend!"

        undoManager.Undo(editor);
        Console.WriteLine(editor.Text); // Output: "" (empty)
    }
}
```

In this example, `TextEditor` is the Originator, `TextMemento` is the Memento, and `UndoManager` is the Caretaker. The `UndoManager` keeps track of the mementos and manages the undo operations, while the `TextEditor` creates and restores its state from mementos.

By using the Memento pattern, you can implement undo/redo funcionality or save the state of objects in a way that maintains encapsulation and keeps the state management code separate from the main application logic.