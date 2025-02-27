# GQL005: Illegal resolver usage

|                        | Value  |
| ---------------------- | ------ |
| **Rule ID**            | GQL004 |
| **Category**           | Usage  |
| **Default severity**   | Error  |
| **Enabled by default** | Yes    |
| **Code fix provided**  | Yes    |

## Cause

A `ResolveXXX` method was invoked for the field that is defined within the
non-output graph type.

## Rule description

Resolvers are only allowed on the output graph types. Output graph types are
types derived from the `ObjectGraphType` class or implementing
`IObjectGraphType` interface.

## How to fix violations

Remove the `ResolveXXX` method call.

## Example of a violation

The following example shows `Resolve` and `ResolveAsync` methods used on the
fields defined within input and interface graph types.

```c#
public class MyInputGraphType : InputObjectGraphType<User>
{
    public MyInputGraphType() =>
        Field<StringGraphType>("Name").Resolve(context => context.Source.Name);
}

public class MyInterfaceGraphType : InterfaceGraphType<Person>
{
    public MyInterfaceGraphType(IStore store) =>
        Field<ListGraphType<NonNullGraphType<PersonGraphType>>>("Children")
            .ResolveAsync(async context => await store.GetChildrenAsync(context.Source.Name));
}
```

## Example of how to fix

```c#
public class MyInputGraphType : InputObjectGraphType<User>
{
    public MyInputGraphType() =>
        Field<StringGraphType>("Name");
}

public class MyInterfaceGraphType : InterfaceGraphType<Person>
{
    public MyInterfaceGraphType(IStore store) =>
        Field<ListGraphType<NonNullGraphType<PersonGraphType>>>("Children");
}
```

## Suppress a warning

If you just want to suppress a single violation, add preprocessor directives to
your source file to disable and then re-enable the rule.

```csharp
#pragma warning disable GQL005
// The code that's violating the rule is on this line.
#pragma warning restore GQL005
```

To disable the rule for a file, folder, or project, set its severity to `none`
in the
[configuration file](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/configuration-files).

```ini
[*.cs]
dotnet_diagnostic.GQL005.severity = none
```

For more information, see
[How to suppress code analysis warnings](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/suppress-warnings).

## Related rules
