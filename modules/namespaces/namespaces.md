---
title: "Namespaces"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

In einem großen Projekt können leicht Hunderte von Klassen entstehen – und irgendwann will vielleicht jemand eine Klasse `Button` in einem GUI-Framework und eine weitere `Button` in einem anderen. Namespaces verhindern solche Kollisionen und helfen dabei, Code logisch zu organisieren.

## Was ist ein Namespace?

Ein Namespace ist ein Organisationsrahmen für Klassen – ähnlich wie Ordner im Dateisystem. Er verhindert Namenskollisionen und strukturiert größere Projekte.

```csharp
namespace HTW.Prog1.Geometrie
{
    class Kreis { ... }
    class Rechteck { ... }
}

namespace HTW.Prog1.Statistik
{
    class Datensatz { ... }
}
```

## `using`-Direktiven

Mit `using` muss der volle Namespace-Pfad nicht jedes Mal geschrieben werden:

```csharp
using System;           // Console, Math, ...
using System.IO;        // File, Directory, ...
using System.Collections.Generic; // List, Dictionary, ...

Console.WriteLine("Hallo"); // statt System.Console.WriteLine(...)
```

## Vollständig qualifizierter Name

Ohne `using` muss der komplette Pfad angegeben werden:

```csharp
System.Console.WriteLine("Hallo");
System.Collections.Generic.List<int> zahlen = new();
```

## Alias bei Namenskonflikten

```csharp
using WinForms = System.Windows.Forms;
using WPF = System.Windows;

WinForms.Button b1 = new WinForms.Button();
WPF.Controls.Button b2 = new WPF.Controls.Button();
```

## File-scoped Namespaces (C# 10+)

Moderne, kompakte Schreibweise ohne extra Einrückung:

```csharp
namespace HTW.Prog1.Geometrie;  // gilt für die gesamte Datei

class Kreis
{
    public double Radius { get; set; }
}
```

## Weitere Quellen

- [Namespaces – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/namespace)
- [using-Direktive – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/using-directive)
