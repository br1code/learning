# Verify

Verify is a snapshot testing library for .NET that simplifies the process of creating and maintaining snapshot tests. It serializes the test output and compares it with the stored snapshot, providing detailed information about any differences. Verify works with various test frameworks, such as xUnit, NUnit, and MSTest.

To start using Verify, install the `Verify.Xunit`, `Verify.NUnit`, or `Verify.MSTest` NuGet package, depending on your preferred test framework.

Here's an example of using Verify to create a snapshot test for a hypothetical e-commerce application with the following components:

- A `Product` class that represents a product with properties like `Id`, `Name`, and `Price`.
- A `ProductService` class that retrieves product information and applies various transformations, such as adding a discount or formatting the product's description.

First, define the `Product` and `ProductService` classes:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Description { get; set; }
}

public class ProductService
{
    public Product ApplyDiscount(Product product, decimal discountPercentage)
    {
        return new Product
        {
            Id = product.Id,
            Name = product.Name,
            Price = product.Price * (1 - discountPercentage / 100),
            Description = product.Description
        };
    }
}
```

Next, create a snapshot test for the `ApplyDiscount` method using Verify and xUnit:

```csharp
using VerifyXunit;
using Xunit;

// Use the VerifyBase attribute to indicate that this test class uses snapshot testing with Verify
public class ProductServiceTests : VerifyBase
{
    private readonly ProductService _productService;

    public ProductServiceTests()
    {
        _productService = new ProductService();
    }

    [Fact]
    public async Task ApplyDiscountTest()
    {
        var product = new Product
        {
            Id = 1,
            Name = "Gaming Laptop",
            Price = 1200.00M,
            Description = "A high-performance gaming laptop with a powerful graphics card and processor."
        };

        var discountedProduct = _productService.ApplyDiscount(product, 10);

        // Use the Verify method from the VerifyBase class to create a snapshot test
        await Verify(discountedProduct);
    }
}
```

When you run the `ApplyDiscountTest` for the first time, Verify will generate a snapshot file named `ProductServiceTests.ApplyDiscountTest.verified.json` in the test project's directory. This file contains a serialized representation of the `discountedProduct` object.

If you make changes to the `ApplyDiscount` method, such as modifying the discount calculation, the test will fail if the output no longer matches the stored snapshot. Verify will provide detailed information about the differences between the expected and actual output, allowing you to decide whether to update the snapshot or fix the code.

In this example, Verify simplifies the process of creating and maintaining snapshot tests for the `ProductService` class. By leveraging snapshot testing, you can ensure that your application's behavior remains consistent as you make changes to the code.

Remember that Verify supports various serializers and comparison methods, making it suitable for testing different types of output, such as XML, JSON, or even images. You can also extend Verify with custom serializers and comparison methods to suit your specific testing needs.