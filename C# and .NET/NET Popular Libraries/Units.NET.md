# Units.NET

https://www.nuget.org/packages/UnitsNet

## Simple Explanation

Units.NET is a package that provides a type-safe way of working with units of measurement in .NET. It allows you to perform mathematical operations on different units of measurement and perform conversions between them easily.

## Deep Explanation

Units.NET is a powerful library that makes it easy to work with units of measurement in .NET. It provides a type-safe way of defining and working with different units of measurement, such as length, mass, time, and more. Units.NET allows you to perform mathematical operations between different units of measurement, as well as convert between them.

Units.NET defines a variety of different units of measurement, such as meters, seconds, kilograms, and more. These units can be combined to form more complex units, such as kilometers per hour or newton-meters. Units.NET also provides support for working with imperial units, such as feet and pounds.

One of the key benefits of Units.NET is its type safety. When you define a quantity with Units.NET, you specify the type of the quantity (such as length or mass) and the unit of measurement (such as meters or kilograms). This allows the compiler to enforce correct usage of units throughout your code, preventing common errors such as accidentally adding two quantities with different units.

## Examples

Here's a simple example of using Units.NET to perform a conversion between different units of length:

```C#
using UnitsNet;

// Convert 10 meters to feet
Length lengthInMeters = Length.FromMeters(10);
Length lengthInFeet = lengthInMeters.ToFeet();

Console.WriteLine($"{lengthInMeters.Meters} meters is {lengthInFeet.Feet} feet");
```

---

Convert temperature from Celsius to Fahrenheit:

```C#
var temperatureC = Temperature.FromDegreesCelsius(20);
var temperatureF = temperatureC.ToDegreesFahrenheit();
Console.WriteLine($"{temperatureC} in Celsius is {temperatureF} in Fahrenheit.");
```

---

Calculate the speed of an object in miles per hour:

```C#
var distanceM = Length.FromMiles(10);
var timeH = Duration.FromHours(1);
var speed = distanceM.Divide(timeH).ToMilesPerHour();
Console.WriteLine($"The object is moving at {speed} miles per hour.");
```

---

Calculate the area of a rectangle in square meters:

```C#
var length = Length.FromMeters(5);
var width = Length.FromMeters(10);
var area = length.Multiply(width).ToSquareMeters();
Console.WriteLine($"The area of the rectangle is {area} square meters.");
```

---

Calculate the force required to lift an object weighing 50 kilograms:

```C#
var weight = Mass.FromKilograms(50);
var acceleration = Acceleration.FromStandardGravity();
var force = weight.Multiply(acceleration).ToNewtons();
Console.WriteLine($"The force required to lift the object is {force} Newtons.");
```