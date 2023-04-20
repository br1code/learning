# Singleton

![](../Assets/singleton.png)

## Simple Explanation

The Singleton design pattern is a creational design pattern that ensures a class has only one instance and provides a global point of access to that instance. It is used when you want to ensure that a class is instantiated only once and that the single instance is easily accessible.

## Deep Explanation

The Singleton pattern involves two main components:

1. A private static instance of the Singleton class.

2. A public static method (typically named "GetInstance" or "Instance") to access the Singleton instance. This method is responsible for creating the instance if it doesn't exist and returning the existing instance if it does.

The Singleton pattern prevents direct instantiation of the class by making its constructor private. This ensures that the class can only be instantiated through the public static method. The pattern is particularly useful when you need a single instance of a class to coordinate actions across multiple parts of your application.

## Examples

Let's imagine we have a simple configuration manager utility that manages application settings.

1. Implement the Singleton class:

```C#
public class ConfigurationManager
{
    private static readonly Lazy<ConfigurationManager> _instance = new Lazy<ConfigurationManager>(() => 
        new ConfigurationManager());

    private ConfigurationManager() { }

    public static ConfigurationManager Instance
    {
        get { return _instance.Value; }
    }

    public string GetSetting(string key)
    {
        // Retrieve the setting value from a data source (e.g., a configuration file)
        // For simplicity, we'll return a hard-coded value here.
        return "SettingValue";
    }
}

```
In this example, we use the `Lazy<T>` class provided by .NET to implement lazy initialization. The `_instance` field is now of type `Lazy<ConfigurationManager>`, and it is initialized with a delegate that creates a new `ConfigurationManager` instance. The `Instance` property returns the value of the `_instance` field, which is the actual Singleton instance.

The `Lazy<T>` class handles thread-safety and ensures that the Singleton instance is created only once when first accessed. This approach can improve performance and reduce memory usage in scenarios where the instance is not always needed.

2. Use the Singleton instance:

```C#
class Program
{
    static void Main(string[] args)
    {
        string settingValue = ConfigurationManager.Instance.GetSetting("SettingKey");
        Console.WriteLine($"The setting value is: {settingValue}");
    }
}
```

In the `Main` method, the Singleton instance of `ConfigurationManager` is accessed through the `Instance` property, and the `GetSetting` method is called to retrieve a setting value.

By using the Singleton pattern, you can ensure that a class has **only one instance**, and that the instance is easily accessible throughout your application. This promotes consistency and helps avoid potential issues caused by multiple instances of a class that should have only one.