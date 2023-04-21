# Flyweight

![](../Assets/flyweight.png)

## Simple Explanation

The Flyweight design pattern is a structural pattern that helps optimize memory usage and improve performance in situations where a large number of objects with similar properties are used. The pattern aims to reduce the number of objects created by sharing common parts of the object state among multiple instances.

## Deep Explanation

The Flyweight pattern achieves memory optimization by dividing an object's state into intrinsic (shared) and extrinsic (unique) states. Intrinsic state is the information that can be shared among instances, while extrinsic state is the information that is specific to each instance.

Flyweight objects are typically immutable and can be safely shared among multiple clients. A Flyweight Factory is often used to manage the creation and sharing of flyweight objects, ensuring that a flyweight object is reused if an existing one with the same intrinsic state is available.

## Examples

Consider an example where we have a text editor that displays characters on the screen. Each character has a font, size, and color. To optimize memory usage, we can use the Flyweight pattern to share font, size, and color information among characters that have the same properties.

1. Define the Flyweight class:

```C#
public class CharacterStyle
{
    public string Font { get; }
    public int Size { get; }
    public ConsoleColor Color { get; }

    public CharacterStyle(string font, int size, ConsoleColor color)
    {
        Font = font;
        Size = size;
        Color = color;
    }

    public void Display(char character)
    {
        Console.WriteLine($"Character: {character}, Font: {font}, Size: {size}, Color: {color}")
    }
}
```

2. Create the Flyweight Factory:

```C#
public class CharacterStyleFactory
{
    private readonly Dictionary<string, CharacterStyle> _characterStyles;

    public CharacterStyleFactory()
    {
        _characterStyles = new Dictionary<string, CharacterStyle>();
    }

    public CharacterStyle GetCharacterStyle(string font, int size, ConsoleColor color)
    {
        string key = $"{font}-{size}-{color}";
        if (!_characterStyles.TryGetValue(key, out CharacterStyle characterStyle))
        {
            characterStyle = new CharacterStyle(font, size, color);
            _characterStyles[key] = characterStyle;
        }
        return characterStyle;
    }
}
```

3. Use the Flyweight pattern:

```C#
class Program
{
    static void Main(string[] args)
    {
        var factory = new CharacterStyleFactory();

        var style1 = factory.GetCharacterStyle("Arial", 12, ConsoleColor.Red);
        var style2 = factory.GetCharacterStyle("Arial", 12, ConsoleColor.Red);
        var style3 = factory.GetCharacterStyle("Arial", 14, ConsoleColor.Blue);

        style1.Display('A');
        style2.Display('B');
        style3.Display('C');
    }
}
```

In this example, `CharacterStyle` is the flyweight class that holds the intrinsic state (font, size, and color), and the `CharacterStyleFactory` manages the creation and sharing of `CharacterStyle` objects. 

The `Main` method in the `Program` class demonstrates how the Flyweight pattern is used to share `CharacterStyle` instances among different characters.

The `CharacterStyleFactory` acts as a cache for `CharacterStyle` objects. It stores the already created `CharacterStyle` instances in a dictionary and reuses them when a client requests a style with the same font, size, and color. This helps to optimize memory usage and performance by reducing the number of objects created and sharing the common parts of the object state among multiple instances.

By using the Flyweight pattern, we can reduce memory usage and improve performance in situations where a large number of objects with similar properties are used.