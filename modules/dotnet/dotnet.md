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

## Kernbestandteile

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

## Installation

Das .NET SDK muss nicht separat installiert werden – es ist in **Visual Studio Community** enthalten. Bei der Installation einfach die Workload **.NET-Desktopentwicklung** auswählen.

- [Visual Studio Community](https://visualstudio.microsoft.com/de/vs/community/) – empfohlen für die Vorlesung
- Alternativ: [Visual Studio Code](https://code.visualstudio.com/) mit C#-Erweiterung ([Einrichtungsanleitung](https://learn.microsoft.com/de-de/dotnet/core/tutorials/with-visual-studio-code))
- Nur SDK (ohne IDE): [dotnet.microsoft.com](https://dotnet.microsoft.com/en-us/download)

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

## .NET-Versionen

| Version | Typ | Hinweis |
| :--- | :--- | :--- |
| .NET 8 | LTS (Long-Term Support) | Empfohlen für neue Projekte |
| .NET 9 | Standard | Aktuellste Version |
| .NET Framework | Legacy | Nur Windows, nicht mehr aktiv entwickelt |

Für diese Veranstaltung verwenden wir **.NET 8** oder neuer. Die Installationsanleitung findet sich auf [dotnet.microsoft.com](https://dotnet.microsoft.com/en-us/download).
{: .notice--primary}
