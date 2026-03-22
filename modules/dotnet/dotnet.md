---
title: "Die .NET-Plattform"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

## Was ist .NET?

**.NET** ist eine freie, plattformübergreifende Laufzeit- und Entwicklungsplattform von Microsoft. Sie bildet die Grundlage für C#-Programme und stellt alles bereit, was zur Ausführung von Code benötigt wird.

**CLR – Common Language Runtime**
Die CLR ist die Laufzeitumgebung von .NET. Sie übernimmt:
- **JIT-Kompilierung:** Übersetzung von CIL in nativen Maschinencode
- **Speicherverwaltung:** Automatische Freigabe nicht mehr benötigter Objekte (Garbage Collection)
- **Typsicherheit:** Überprüfung von Typen zur Laufzeit
- **Ausnahmebehandlung:** Einheitliches System für Laufzeitfehler

**BCL – Base Class Library**
Die BCL ist die Standardbibliothek von .NET. Sie enthält fertige Klassen für:
- Ein- und Ausgabe (`System.Console`, `System.IO`)
- Datenstrukturen (`System.Collections.Generic`)
- Textverarbeitung (`System.String`, `System.Text`)
- Netzwerk, Kryptographie, und vieles mehr

**SDK – Software Development Kit**
Das .NET SDK enthält Compiler, Build-Tools und die CLI (`dotnet`-Befehl) für die Entwicklung.

## .NET auf der Kommandozeile

```bash
# Neues Konsolenprojekt anlegen
$ dotnet new console -n MeinProjekt

# In das Projektverzeichnis wechseln
$ cd MeinProjekt

# Programm kompilieren und ausführen
$ dotnet run

# Nur kompilieren (ohne Ausführen)
$ dotnet build
```

## Sprachen auf .NET

C# ist nicht die einzige Sprache, die auf .NET läuft. Die Plattform unterstützt mehrere Sprachen, die alle dieselbe CIL erzeugen und damit dieselbe BCL und Runtime nutzen:

| Sprache | Charakter | Typischer Einsatz |
| :--- | :--- | :--- |
| **C#** | Objektorientiert, modern | Allgemein, Web, Desktop, Spiele |
| **F#** | Funktional | Datenanalyse, wissenschaftliches Rechnen |
| **VB.NET** | BASIC-Syntax | Legacy-Anwendungen, kaum noch neu |

Weil alle dieselbe CIL erzeugen, kann eine C#-Bibliothek problemlos aus F# genutzt werden – und umgekehrt. Die Sprachwahl ist eine Frage des Stils und des Einsatzzwecks, nicht der Plattform.

## .NET-Versionen

.NET erscheint im Jahrestakt. Gerade Versionsnummern sind **LTS** (Long-Term Support, 3 Jahre Support), ungerade Versionen laufen nur 18 Monate:

| Version | Typ | Status |
| :--- | :--- | :--- |
| .NET 8 | LTS | Unterstützt bis Nov 2026 |
| .NET 9 | Standard | Aktuell |
| .NET 10 | LTS | ab Nov 2025 |
| .NET Framework | Legacy | Nur Windows, kein Nachfolger |

Jede .NET-Version bringt neue C#-Sprachfeatures mit. Code, der neuere Features nutzt, lässt sich nicht mit einer älteren SDK-Version kompilieren. Das `<TargetFramework>`-Element in der `.csproj`-Datei legt fest, welche Version ein Projekt erwartet:

```xml
<TargetFramework>net9.0</TargetFramework>
```

In gemeinsamen Projekten – z.B. im Team oder wenn Studierende gegenseitig Code austauschen – muss dieselbe SDK-Version installiert sein, sonst schlägt `dotnet build` fehl. Die installierte Version lässt sich prüfen mit:

```bash
$ dotnet --version
9.0.203
```

Es ist ohne Weiteres möglich, dass auf einem System mehrere .NET-Versionen installiert sind.
## Weitere Quellen

- [.NET-Plattform – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/core/introduction)
- [.NET SDK herunterladen](https://dotnet.microsoft.com/en-us/download)
- [F# – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/fsharp/what-is-fsharp)
