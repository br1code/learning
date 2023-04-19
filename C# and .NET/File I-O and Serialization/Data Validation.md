# Data Validation

## Explanation

Data validation is an important aspect of processing data in any format, including CSV, JSON, and XML. One way to validate data is to use schema validation, which checks whether the data conforms to a predefined schema.

For CSV data, a simple way to validate the data is to check if each row has the expected number of columns and if each column has the expected data type. For example, if a CSV file is expected to have five columns, and the fourth column is expected to be a number, the validation process can check that each row has exactly five columns and that the fourth column of each row can be successfully converted to a number.

For JSON and XML data, schema validation can be used to check the structure and data types of the data. JSON and XML schema validation can be performed using JSON Schema or XML Schema, respectively. These schema languages define a set of rules that the data must follow, such as specifying the required structure of the data, the data types of each element, and constraints on the allowed values.

Another way to validate data is to perform custom validation checks based on business logic or domain-specific rules. For example, in a CSV file that contains customer data, a custom validation check can ensure that each customer's age is greater than or equal to 18 years old. Similarly, for JSON or XML data, custom validation checks can be performed to ensure that the data meets the requirements of the specific application or domain.

## Examples

1 - Here's a simple code example of validating a CSV file in C#:

```C#
using System.IO;
using System.Text.RegularExpressions;

// Define the regular expression pattern for a valid CSV line
Regex csvLinePattern = new Regex(@"^[^,]*(,[^,]*)*$");

// Open the CSV file for reading
using (StreamReader reader = new StreamReader("example.csv"))
{
    // Loop through each line in the file
    string line;
    while ((line = reader.ReadLine()) != null)
    {
        // Check if the line matches the expected CSV format
        if (!csvLinePattern.IsMatch(line))
        {
            Console.WriteLine("Error: Invalid CSV format in line: " + line);
        }
    }
}
```

2 -  Here's an example of validating JSON using the System.Text.Json namespace in C#:

```C#
using System;
using System.Text.Json;
using System.Text.Json.Serialization;
using System.Collections.Generic;

public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        string json = @"{
            ""name"": ""John"",
            ""age"": 30
        }";

        var options = new JsonSerializerOptions
        {
            PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
            IgnoreNullValues = true,
            Converters = { new JsonStringEnumConverter(JsonNamingPolicy.CamelCase) }
        };

        try
        {
            Person person = JsonSerializer.Deserialize<Person>(json, options);

            if (person != null && person.Age > 0)
            {
                Console.WriteLine($"Name: {person.Name}, Age: {person.Age}");
            }
            else
            {
                Console.WriteLine("Invalid JSON data.");
            }
        }
        catch (JsonException ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

3 - Here's a simple code example of validating an XML file against an XSD schema using the XmlReader and XmlReaderSettings classes:

```C#
using System;
using System.Xml;
using System.Xml.Schema;

class Program
{
    static void Main(string[] args)
    {
        try
        {
            XmlReaderSettings settings = new XmlReaderSettings();
            settings.ValidationType = ValidationType.Schema;
            settings.Schemas.Add(null, "schema.xsd");
            settings.ValidationEventHandler += new ValidationEventHandler(ValidationCallBack);

            XmlReader reader = XmlReader.Create("file.xml", settings);
            while (reader.Read()) ;
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

    private static void ValidationCallBack(object sender, ValidationEventArgs e)
    {
        Console.WriteLine("Validation Error: {0}", e.Message);
    }
}
```