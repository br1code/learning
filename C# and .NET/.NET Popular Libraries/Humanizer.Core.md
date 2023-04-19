# Humanizer.Core

https://www.nuget.org/packages/Humanizer.Core

## Simple Explanation

Humanizer.Core is a package that provides human-friendly string extensions for .NET. It allows you to convert strings to a more readable and user-friendly format, such as converting camelCase or PascalCase to words, formatting numbers and dates in a more human-readable way, and more.

## Deep Explanation

Humanizer.Core is an open-source package that is available on NuGet, and is built for .NET Standard and .NET Core. It provides a set of extension methods for the String and Object classes, allowing you to convert strings to a more human-readable format. It also provides a variety of formatting options for numbers, dates, and times.

Some of the key features of Humanizer.Core include:

- String extensions for converting camelCase, PascalCase, and snake_case to words, such as "SomeCamelCaseString" to "Some Camel Case String".

- String extensions for truncating text, pluralizing and singularizing words, and more.

- Formatting extensions for numbers, including spell out, rounding, and precision formatting.

- Formatting extensions for dates and times, including relative time formatting (e.g. "3 days ago"), and duration formatting (e.g. "1 hour, 30 minutes").

## Examples

```C#
using Humanizer;

// Convert camelCase to words
string camelCaseString = "someCamelCaseString";
string words = camelCaseString.Humanize(); // "some camel case string"

// Format a number as words
int number = 123456;
string numberWords = number.ToWords(); // "one hundred and twenty-three thousand, four hundred and fifty-six"

// Format a date as relative time
DateTime date = DateTime.Now.AddDays(-3);
string relativeTime = date.Humanize(); // "3 days ago"
```

These are just a few examples of what you can do with Humanizer.Core. The package provides many more useful extensions for working with strings, numbers, dates, and times, and can be a great tool for making your code more user-friendly and readable.