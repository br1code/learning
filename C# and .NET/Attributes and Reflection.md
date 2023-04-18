# Attributes and Reflection

## Simple Explanation

Attributes are markers that you can add to your code to provide additional information about the code.

Reflection is a feature of C# that allows you to inspect and interact with code at runtime.

## Deep Explanation

Attributes are a way to add metadata to your code. They provide additional information about the code that the compiler can use to generate warnings, errors, or optimizations. Attributes can be added to classes, methods, properties, fields, and other parts of your code. For example, you can use the [Obsolete] attribute to mark a method that should no longer be used, and the compiler will generate a warning when that method is used.

Attributes can be used in various types of elements in C#, such as:

- Classes, interfaces, and structs
- Methods, constructors, and properties
- Fields, events, and delegates
- Parameters and return values
- Assembly-level attributes

Reflection is a feature of C# that allows you to inspect and interact with code at runtime. With reflection, you can examine the metadata of a type, such as its name, methods, properties, and attributes. You can also create instances of types and invoke their methods or access their properties dynamically. Reflection is often used in frameworks that provide generic functionality, such as serialization or dependency injection, where the framework needs to dynamically create instances of types and invoke their methods or access their properties.

## Examples

Use Attributes:

```C#
[Obsolete("This method is deprecated. Use the new method instead.")]
public void OldMethod()
{
    // implementation here
}

public void NewMethod()
{
    // implementation here
}
```

Invoke a method of a Class:

```C#
Type type = typeof(MyClass);
MethodInfo methodInfo = type.GetMethod("MyMethod");
object instance = Activator.CreateInstance(type);
methodInfo.Invoke(instance, null);
```

List all the methods of a Class:

```C#
using System;
using System.Reflection;

class MyClass
{
    public void Method1(string arg1, int arg2)
    {
        // method implementation
    }

    public int Method2(double arg1)
    {
        // method implementation
        return 0;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Type type = typeof(MyClass);
        MethodInfo[] methods = type.GetMethods();

        foreach (MethodInfo method in methods)
        {
            Console.WriteLine("Name: {0}", method.Name);
            Console.WriteLine("Return type: {0}", method.ReturnType.Name);

            ParameterInfo[] parameters = method.GetParameters();
            Console.WriteLine("Parameters:");
            foreach (ParameterInfo parameter in parameters)
            {
                Console.WriteLine("  {0} ({1})", parameter.Name, parameter.ParameterType.Name);
            }
            Console.WriteLine();
        }
    }
}
```

## Most common used Attributes in C# and .NET

- [Serializable]: Indicates that a class can be serialized (i.e., converted to a stream of bytes for storage or transmission).

- [Obsolete]: Marks a method or class as deprecated, and provides a message explaining why it should no longer be used.

- [DataContract]: Used to indicate that a class is part of a data contract, which is a way to serialize and deserialize data.

- [DataMember]: Specifies that a property or field should be included when serializing or deserializing a data contract.

- [Required]: Indicates that a property or field is required.

- [StringLength]: Specifies the maximum length of a string property or field.

- [Range]: Specifies the valid range for a numeric property or field.

- [Conditional]: Specifies that a method should only be called if a certain condition is met.

- [DllImport]: Used to import functions from a DLL into a C# program.

- [WebMethod]: Indicates that a method is part of a web service.