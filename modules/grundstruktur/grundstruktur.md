---
title: "Grundstruktur eines C#-Programms"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bevor man eigene Programme schreibt, lohnt es sich zu verstehen, wie ein C#-Programm grundsätzlich aufgebaut ist. Zwei Schreibweisen sind heute gebräuchlich – eine klassische und eine moderne.

## Klassische Schreibweise

```csharp
using System;

namespace MeinErstesProgramm
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hallo, Welt!");
        }
    }
}
```

Die einzelnen Bestandteile haben klare Aufgaben:

- **`using System;`** – importiert den Namensraum `System`, der Standardklassen wie `Console` enthält
- **`namespace`** – organisiert Code in logische Gruppen und vermeidet Namenskollisionen
- **`class Program`** – in C# muss jeder Code in einer Klasse stehen
- **`static void Main`** – der Einstiegspunkt: hier beginnt die Ausführung des Programms

Was `class`, `static` und `void` genau bedeuten, wird erst im Laufe der Vorlesung klar — insbesondere wenn es um objektorientiertes Programmieren und Methoden geht. Für den Moment reicht es, die klassische Struktur als festes Gerüst zu akzeptieren und den eigenen Code innerhalb von `Main` zu schreiben.

## Moderne Schreibweise: Top-Level Statements (ab .NET 6)

Ab .NET 6 darf man für einfache Programme auf `namespace`, `class` und `Main` verzichten – der Compiler ergänzt diese Struktur automatisch im Hintergrund. Diese Kurzform heißt offiziell **Top-Level Statements**:

```csharp
Console.WriteLine("Hallo, Welt!");
```

Hier fällt auf, dass auch die Zeile `using System;` fehlt. Das liegt daran, dass neue .NET-Projekte standardmäßig **Implicit Usings** aktiviert haben – häufig benötigte Namensräume wie `System` werden automatisch importiert, ohne dass man sie hinschreiben muss. In der Projektdatei (`.csproj`) steht dafür:

```xml
<ImplicitUsings>enable</ImplicitUsings>
```

Das Ergebnis ist identisch zur klassischen Form. In dieser Vorlesung verwenden wir beide Formen: die kurze Schreibweise für schnelle Experimente, die klassische für größere Programme, bei denen die Struktur hilft.
{: .notice--primary}

## Weitere Quellen

- [Einführung in C# – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/tour-of-csharp/)
- [Struktur eines C#-Programms – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/program-structure/)
