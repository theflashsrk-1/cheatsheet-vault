#codereview #csharp #language-reference

# Introduction to C# (Reference)

Language fundamentals (conceptual module). Related: [whitebox](whitebox.md) · [deserialization](../Web/deserialization.md)

## Contents
- [Basics](#basics)
- [Variables / Constants / Enums](#variables--constants--enums)
- [Operators](#operators)
- [Security-Relevant Sinks](#security-relevant-sinks)

## Basics
```csharp
static void Main(string[] args) { }
```
Program entry point. C# is case-sensitive; statements end with `;`.

## Variables / Constants / Enums
```csharp
int num;
const double Pi = 3.14159;
enum Days { Sun, Mon, Tue, Wed, Thu, Fri, Sat };
```
Declarations. Core types: `int`, `double`, `char`, `bool`, `string`.

## Operators
```csharp
+ - * / %        // arithmetic
== != < > <= >=  // relational
&& || !          // logical
& | ^ ~ << >>    // bitwise
```
Operator groups.

## Security-Relevant Sinks
```csharp
System.Diagnostics.Process.Start(...)   // command execution
BinaryFormatter.Deserialize(...)        // unsafe deserialization
```
Review targets. See [deserialization](../Web/deserialization.md).
