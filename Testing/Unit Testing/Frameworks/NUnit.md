# NUnit

NUnit is a widely used, open-source unit testing framework for .NET applications. It provides a robust and flexible platform for writing and executing tests, with a focus on extensibility, readability, and ease of use. NUnit has been a popular choice among .NET developers for many years and has influenced the design of other testing frameworks, such as xUnit.

Here are some key concepts and features of NUnit:

1. Test Classes: Test classes in NUnit are any public classes marked with the `[TestFixture]` attribute. They contain test methods, which represent individual test cases.

2. Test Methods: Test methods are methods marked with the `[Test]` attribute. These methods should be public, return void, and take no parameters. Test methods contain the test logic, including the Arrange, Act, and Assert phases.

3. Test Assertions: NUnit provides a set of built-in assertion methods through the `Assert` class, which is used to verify test results.

4. Test Runners: NUnit supports various test runners, including console, Visual Studio, and other third-party runners. These runners discover and execute tests and provide a way to visualize test results.

5. Test Lifecycle: NUnit supports constructor and method-level setup and teardown using the `[OneTimeSetUp]`, `[OneTimeTearDown]`, `[SetUp]`, and `[TearDown]` attributes. These methods help set up and clean up any resources or state required for the tests.

6. Parameterized Tests: NUnit supports parameterized tests using the `[TestCase]` attribute, which allows you to pass different input values to a test method. This helps to test a broader range of scenarios with less code.

7. Categories and Ignore: NUnit allows you to categorize tests using the `[Category]` attribute, which can be helpful for organizing and filtering tests. You can also ignore a test using the `[Ignore]` attribute, which is useful for temporarily disabling a test.

Let's look at an example of a test class in NUnit, using a Calculator class:

```csharp
using NUnit.Framework;

public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}

[TestFixture]
public class CalculatorTests
{
    private Calculator _calculator;

    [OneTimeSetUp]
    public void OneTimeSetUp()
    {
        _calculator = new Calculator();
    }

    [Test]
    public void Add_TwoNumbers_ShouldReturnCorrectSum()
    {
        // Arrange
        int a = 5;
        int b = 3;
        int expectedResult = 8;

        // Act
        int actualResult = _calculator.Add(a, b);

        // Assert
        Assert.AreEqual(expectedResult, actualResult);
    }

    [TestCase(1, 1, 2)]
    [TestCase(2, 3, 5)]
    [TestCase(-1, 2, 1)]
    public void Add_WithDifferentInputs_ShouldReturnCorrectSums(int a, int b, int expectedResult)
    {
        // Act
        int actualResult = _calculator.Add(a, b);

        // Assert
        Assert.AreEqual(expectedResult, actualResult);
    }
}
```

In this example, we have a test class named `CalculatorTests` with two test methods: `Add_TwoNumbers_ShouldReturnCorrectSum` and `Add_WithDifferentInputs_ShouldReturnCorrectSums`. The first test method uses the `[Test]` attribute, and the second uses the `[TestCase]` attribute for parameterized testing. The `[OneTimeSetUp]` attribute is used to initialize the `Calculator` instance once for all test methods in the class.

NUnit is a powerful and flexible testing framework with a long history and many advanced features. You can find more information and examples in the official documentation: https://docs.nunit.org/articles/nunit/intro