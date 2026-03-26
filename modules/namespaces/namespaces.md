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

Einen Namespace zu definieren ist der erste Schritt — doch wie greift man bequem auf Klassen aus anderen Namespaces zu? Ohne Hilfe müsste man bei jedem Zugriff den vollständigen Pfad angeben — das wäre extrem umständlich. Die `using`-Direktive am Anfang der Datei importiert einen Namespace, sodass man die Klassen darin direkt verwenden kann:

```csharp
using System;           // Console, Math, ...
using System.IO;        // File, Directory, ...
using System.Collections.Generic; // List, Dictionary, ...

Console.WriteLine("Hallo"); // statt System.Console.WriteLine(...)
```

In der Praxis spart man sich durch `using`-Direktiven viel Schreibarbeit und macht den Code deutlich lesbarer. Wie umständlich es ohne wäre, zeigt der folgende Abschnitt.

## Vollständig qualifizierter Name

Ohne `using` muss der komplette Pfad angegeben werden:

```csharp
System.Console.WriteLine("Hallo");
System.Collections.Generic.List<int> zahlen = new();
```

## Alias bei Namenskonflikten

Manchmal importiert man zwei Namespaces, die eine Klasse mit demselben Namen enthalten — zum Beispiel `Button` in verschiedenen UI-Frameworks. In diesem Fall hilft ein Alias, um die Mehrdeutigkeit aufzulösen:

```csharp
using WinForms = System.Windows.Forms;
using WPF = System.Windows;

WinForms.Button b1 = new WinForms.Button();
WPF.Controls.Button b2 = new WPF.Controls.Button();
```

In .NET-Projekten mit `<ImplicitUsings>enable</ImplicitUsings>` (Standard seit .NET 6) werden die gängigsten Namespaces wie `System`, `System.Collections.Generic` und `System.Linq` automatisch importiert — man muss sie nicht mehr manuell angeben.
{: .notice--primary}

## File-scoped Namespaces (C# 10+)

In der bisherigen Schreibweise umschließt der Namespace den gesamten Code mit geschweiften Klammern — das kostet eine Einrückungsebene, obwohl in der Praxis fast immer nur ein Namespace pro Datei verwendet wird. Seit C# 10 gibt es eine kompaktere Schreibweise: Statt den gesamten Code in geschweifte Klammern zu setzen, genügt eine einzige Zeile mit Semikolon am Ende. Sie gilt für die gesamte Datei und spart eine Ebene Einrückung:

```csharp
namespace HTW.Prog1.Geometrie;  // gilt für die gesamte Datei

class Kreis
{
    public double Radius { get; set; }
}
```

Mit `global using` (ebenfalls ab C# 10) kann man eine `using`-Direktive einmalig deklarieren, die dann automatisch in allen Dateien des Projekts gilt — praktisch für häufig genutzte Namespaces wie eigene Hilfsklassen.
{: .notice--info}

## Weitere Quellen

- [Namespaces – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/namespace)
- [using-Direktive – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/using-directive)
