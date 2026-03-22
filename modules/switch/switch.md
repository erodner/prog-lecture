---
title: "switch – Mehrfachverzweigungen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Wenn ein Wert viele verschiedene konkrete Ausprägungen haben kann – Wochentage, Menüoptionen, Befehle – wird eine lange Kette von `else if` schnell unübersichtlich. `switch` ist genau für solche Situationen gedacht und macht den Code deutlich lesbarer.

## Wann `switch` statt `if-else`?

Wenn ein Wert mit vielen konkreten Fällen verglichen wird, ist `switch` übersichtlicher als eine Kette von `else if`.

## Klassisches `switch`

```csharp
int tag = 3;
string name;

switch (tag)
{
    case 1: name = "Montag";     break;
    case 2: name = "Dienstag";   break;
    case 3: name = "Mittwoch";   break;
    case 4: name = "Donnerstag"; break;
    case 5: name = "Freitag";    break;
    default: name = "Wochenende"; break;
}

Console.WriteLine(name); // Mittwoch
```

`break` ist zwingend – ohne es würde die Ausführung in den nächsten Fall „fallen".

## Mehrere Fälle zusammenfassen

```csharp
switch (tag)
{
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        Console.WriteLine("Werktag");
        break;
    case 6:
    case 7:
        Console.WriteLine("Wochenende");
        break;
}
```

## Moderne switch-Expression (C# 8+)

Kompaktere Schreibweise, die direkt einen Wert zurückgibt:

```csharp
string jahreszeit = monat switch
{
    12 or 1 or 2 => "Winter",
    3 or 4 or 5  => "Frühling",
    6 or 7 or 8  => "Sommer",
    9 or 10 or 11 => "Herbst",
    _ => "Ungültiger Monat"
};
```

## switch mit strings

```csharp
string befehl = Console.ReadLine();

switch (befehl.ToLower())
{
    case "start": Console.WriteLine("Programm gestartet."); break;
    case "stop":  Console.WriteLine("Programm gestoppt."); break;
    default:      Console.WriteLine("Unbekannter Befehl."); break;
}
```

Übung: Schreibe mit einer switch-Expression einen einfachen Rechner: Operator (`+`, `-`, `*`, `/`) und zwei Zahlen einlesen, Ergebnis ausgeben.
{: .notice--info}

## Weitere Quellen

- [switch-Anweisung und switch-Ausdruck – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/statements/selection-statements#the-switch-statement)
