# GQL007: Can not set source field

|                        | Value   |
| ---------------------- | ------- |
| **Rule ID**            | GQL007  |
| **Category**           | Usage   |
| **Default severity**   | Warning |
| **Enabled by default** | Yes     |
| **Code fix provided**  | No      |

## Cause

This rule triggers when a field defined on a type deriving from
`InputObjectGraphType<TSourceType>` has a matching field or property on the
`TSourceType` type but it's not not settable. If the type or one of its base
types override the `ParseDictionary` method the validation is skipped.

## Rule description

The `TSourceType` field or property can be set when all the following rules are
respected:

- Must have the exact same name as the input field (case-insensitive)
- Must be public
- Must not be static
- Field must not be constant or readonly
- Property must have a public setter

## How to fix violations

Follow the rules described in the [Rule description](#rule-description) section
or remove the invalid input field.

## Example of a violation

The following example shows input fields with invalid source type fields and
properties:

- The `FirstName` property is private
- The `LastName` property is public but has a private setter
- The `Title` field is static
- The `Age` field is readonly

```c#
public class MyInputGraphType : InputObjectGraphType<MySourceType>
{
    public MyInputGraphType()
    {
        Field<StringGraphType>("FirstName");
        Field<StringGraphType>("LastName");
        Field<StringGraphType>("Title");
        Field<IntGraphType>("Age");
    }
}

public class MySourceType
{
    private string FirstName { get; set; }
    public string LastName { get; private set; }
    public static string Title;
    public readonly string Age;
}
```

## Example of how to fix

Make the source type fields and properties settable

```c#
public class MyInputGraphType : InputObjectGraphType<MySourceType>
{
    public MyInputGraphType()
    {
        Field<StringGraphType>("FirstName");
        Field<StringGraphType>("LastName");
        Field<StringGraphType>("Title");
        Field<IntGraphType>("Age");
    }
}

public class MySourceType
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Title;
    public string Age;
}
```

## Configure the analyzer

If the `ParseDictionary` method of the analyzed type or any of its base types is
overridden, the type analysis is skipped. However, you can manually force the
analysis by specifying a comma-delimited list of full type names in the
`.editorconfig` file using the
`dotnet_diagnostic.input_graph_type_analyzer.force_types_analysis` configuration
key.

For instance, to enforce the analysis check for both
`MyServer.BaseInputObjectGraphType` and `MyServer.BaseInputObjectGraphType2`,
include the following configuration in your `.editorconfig` file:

```ini
dotnet_diagnostic.input_graph_type_analyzer.force_types_analysis = MyServer.BaseInputObjectGraphType,MyServer.BaseInputObjectGraphType2
```

## Suppress a warning

If you just want to suppress a single violation, add preprocessor directives to
your source file to disable and then re-enable the rule.

```csharp
#pragma warning disable GQL007
// The code that's violating the rule is on this line.
#pragma warning restore GQL007
```

To disable the rule for a file, folder, or project, set its severity to `none`
in the
[configuration file](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/configuration-files).

```ini
[*.cs]
dotnet_diagnostic.GQL007.severity = none
```

For more information, see
[How to suppress code analysis warnings](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings).

## Related rules

[GQL006: Can not match input field to the source field](/GQL006_CanNotMatchInputFieldToTheSourceField)
