# GQL001: Define the name in `Field`, `Connection` or `ConnectionBuilder.Create` method

|                        | Value   |
| ---------------------- | ------- |
| **Rule ID**            | GQL001  |
| **Category**           | Usage   |
| **Default severity**   | Warning |
| **Enabled by default** | Yes     |
| **Code fix provided**  | Yes     |

## Cause

The `FieldBuilder` or `ConnectionBuilder` instance was created using the
`Field`, `Connection` or `ConnectionBuilder.Create` method overload that doesn't
require a name argument, with the name being supplied through the `Name` method.

## Rule description

The name should be provided in the `Field`, `Connection` or
`ConnectionBuilder.Create` method. Method overloads without the name argument
are obsolete and will be removed in future version.

## How to fix violations

Use the `Field`, `Connection` or `ConnectionBuilder.Create` method overload that
takes the name argument and remove the call to the `Name` method.

## Example of a violation

```c#
Field<StringGraphType>().Name("Name");
Connection<StringGraphType>().Name("Name");
ConnectionBuilder<string>.Create<StringGraphType>().Name("Name");
ConnectionBuilder.Create<StringGraphType, string>().Name("Name");
```

## Example of how to fix

```c#
Field<StringGraphType>("Name");
Connection<StringGraphType>("Name");
ConnectionBuilder<string>.Create<StringGraphType>("Name");
ConnectionBuilder.Create<StringGraphType, string>("Name");
```

## Suppress a warning

If you just want to suppress a single violation, add preprocessor directives to
your source file to disable and then re-enable the rule.

```csharp
#pragma warning disable GQL001
// The code that's violating the rule is on this line.
#pragma warning restore GQL001
```

To disable the rule for a file, folder, or project, set its severity to `none`
in the
[configuration file](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/configuration-files).

```ini
[*.cs]
dotnet_diagnostic.GQL001.severity = none
```

For more information, see
[How to suppress code analysis warnings](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings).

## Related rules

[GQL002: `Name` method invocation can be removed](/GQL002_NameMethodInvocationCanBeRemoved)  
[GQL003: Different names defined by `Field`, `Connection` or `ConnectionBuilder.Create` and `Name` methods](/GQL003_DifferentNamesDefinedByFieldAndNameMethods)
