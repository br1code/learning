# Interpreter

## Simple Explanation

The Interpreter design pattern is a behavioral design pattern used for defining a domain-specific language or grammar and providing an interpreter to evaluate expressions in that language. This pattern is useful when you need to represent and evaluate complex rules, queries, or expressions that can be easily represented in a textual format.

## Deep Explanation

The Interpreter pattern involves creating an abstract syntax tree (AST) to represent the structure of the domain-specific language. Each node in the AST represents a rule or an operation in the language. The interpreter traverses the AST and evaluates the expressions according to the rules defined by the nodes.

The main components of the Interpreter pattern are:

1. Abstract Expression - an interface or abstract class that defines a method for interpreting expressions.

2. Terminal Expression - a class implementing the Abstract Expression interface, representing terminal (leaf) nodes in the AST.

3. Non-terminal Expression - a class implementing the Abstract Expression interface, representing non-terminal (composite) nodes in the AST.

4. Context - a class containing global information, which may be used by the expressions during interpretation.

## Examples

Imagine we need to evaluate simple arithmetic expressions like "3 + 5" or "7 - 2".

1. Create the Abstract Expression interface:

```C#
public interface IExpression
{
    int Interpret();
}
```

2. Implement Terminal Expression classes:

```C#
public class NumberExpression : IExpression
{
    private readonly int _number;

    public NumberExpression(int number)
    {
        _number = number;
    }

    public int Interpret()
    {
        return _number;
    }
}
```

3. Implement Non-terminal Expression classes:

```C#
public class AddExpression : IExpression
{
    private readonly IExpression _left;
    private readonly IExpression _right;

    public AddExpression(IExpression left, IExpression right)
    {
        _left = left;
        _right = right;
    }

    public int Interpret()
    {
        return _left.Interpret() + _right.Interpret();
    }
}

public class SubtractExpression : IExpression
{
    private readonly IExpression _left;
    private readonly IExpression _right;

    public SubtractExpression(IExpression left, IExpression right)
    {
        _left = left;
        _right = right;
    }

    public int Interpret()
    {
        return _left.Interpret() - _right.Interpret();
    }
}
```

4. Evaluate expressions:

```C#
class Program
{
    static void Main(string[] args)
    {
        IExpression expression1 = new AddExpression(new NumberExpression(3), new NumberExpression(5));
        int result1 = expression1.Interpret();
        Console.WriteLine("3 + 5 = " + result1);

        IExpression expression2 = new SubtractExpression(new NumberExpression(7), new NumberExpression(2));
        int result2 = expression2.Interpret();
        Console.WriteLine("7 - 2 = " + result2);
    }
}
```

In this example, `NumberExpression` represents a terminal expression (a number) and `AddExpression` and `SubtractExpression` represent non-terminal expressions (addition and subtraction operations). The expressions are combined into an abstract syntax tree and interpreted to produce the final result. This pattern can be extended to support more complex expressions and operations as needed.