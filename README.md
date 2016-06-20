# Types.Hexadecimal

[![Build](https://ci.appveyor.com/api/projects/status/4nj8qmykpc7fulov?svg=true)](https://ci.appveyor.com/project/skthomasjr/arguments)
[![Release](https://img.shields.io/github/release/skthomasjr/Arguments.svg?maxAge=2592000)](https://github.com/skthomasjr/Arguments/releases)
[![NuGet](https://img.shields.io/nuget/v/Arguments.NET.svg)](https://www.nuget.org/packages/Arguments.NET)
[![License](https://img.shields.io/github/license/skthomasjr/Arguments.svg?maxAge=2592000)](LICENSE.md)
[![Author](https://img.shields.io/badge/author-Scott%20K.%20Thomas%2C%20Jr.-blue.svg?maxAge=2592000)](https://www.linkedin.com/in/skthomasjr)
[![Join the chat at https://gitter.im/skthomasjr/Arguments](https://badges.gitter.im/skthomasjr/Arguments.svg)](https://gitter.im/skthomasjr/Arguments?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Types.Hexadecimal is a library for interacting with command-line arguments passed into a .NET application. The library features a fluent interface and has provisions for inversion of control (IoC).

The syntax is easy to read and manage and frees the implementer from plumbing issues such as case-sensative comparisons, argument separators, and argument processing flow and chaining.

```C#
/// This code will process arguments "-a:5" or "/d" or "--dee".
static int Main(string[] args)
{
  ArgumentManager.Initialize(args)
    .AddArgument("a")
      .WithAction(parameter => { Console.WriteLine($"Argument 'a' processed with value: {parameter}"); })
    .AddArgument("d", "dee")
      .WithAction(parameter => { Console.WriteLine("Argument 'd' processed"); })
    .Process()
}
```

An argument implementation can be injected by implementing the IArgunet interface.

```C#
public class SampleInjectedArgument : IArgument
{
  public SampleInjectedArgument()
  {
    Action = (target, parameter) => { ProcessAction((int)target, parameter); };
  }

  public Action<object, string> Action { get; set; }

  public string[] Flags { get; set; } = { "y" };

  public object Target { get; set; } = 55;

  public bool TerminateAfterExecution { get; set; } = false;

  private void ProcessAction(int target, string parameter)
  {
    Console.WriteLine(target * 1000);
  }
}
```
A more detailed sample project is included with the source.
