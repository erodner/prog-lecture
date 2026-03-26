---
title: "Felder und Properties"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Daten einfach als öffentliche Variablen in einer Klasse zu speichern klingt bequem – aber dann kann jeder von außen beliebige Werte setzen, auch ungültige. Properties lösen dieses Problem: sie bieten kontrollierten Zugriff auf interne Daten und erlauben Validierungslogik direkt beim Lesen oder Schreiben.

## Felder (fields)

Felder sind Variablen, die direkt in einer Klasse deklariert werden:

```csharp
class Auto
{
    public string Marke;   // öffentliches Feld
    private int baujahr;   // privates Feld – nur innerhalb der Klasse zugreifbar
}
```

Öffentliche Felder direkt zu exponieren gilt als schlechter Stil, weil keine Kontrolle über ungültige Werte möglich ist.
{: .notice--warning}

Wie lässt sich der Zugriff auf Daten kontrollieren, ohne die bequeme Syntax aufzugeben? Genau hier kommen Properties ins Spiel.

## Properties (Eigenschaften)

Properties sehen von außen aus wie Felder (`konto.Kontostand`), ermöglichen aber intern Logik beim Lesen und Schreiben. Sie bestehen aus einem `get`-Accessor (zum Lesen) und einem optionalen `set`-Accessor (zum Schreiben):

```csharp
class Konto
{
    private double kontostand;

    public double Kontostand
    {
        get { return kontostand; }
        private set { kontostand = value; } // nur intern setzbar
    }

    public void Einzahlen(double betrag)
    {
        if (betrag <= 0) throw new ArgumentException("Betrag muss positiv sein.");
        kontostand += betrag;
    }
}
```

Das Schlüsselwort `value` im `set`-Accessor enthält automatisch den Wert, der zugewiesen wird — man muss ihn nicht als Parameter deklarieren. Mit `private set` kann der Wert nur innerhalb der Klasse gesetzt werden, von außen ist er nur lesbar.

## Auto-Properties (Kurzschreibweise)

In vielen Fällen braucht man keine Validierungslogik — man will nur ein Feld mit kontrolliertem Zugriff. Für diese Situationen gibt es **Auto-Properties**: Der Compiler erzeugt automatisch ein verstecktes Hintergrundfeld (engl. *backing field*):

```csharp
class Person
{
    public string Name { get; set; }
    public int Alter { get; set; }
    public string Email { get; private set; }  // nur intern setzbar
}

var p = new Person { Name = "Anna", Alter = 21 };
Console.WriteLine(p.Name); // Anna
```

Die Syntax `{ get; set; }` ist die Kurzform — kein Hintergrundfeld und kein `return` nötig. `{ get; private set; }` erlaubt das Lesen von überall, das Schreiben aber nur innerhalb der Klasse. Die Initialisierung mit `new Person { Name = "Anna", Alter = 21 }` heißt **Objekt-Initialisierer** und setzt die Properties direkt nach der Erstellung.

Auto-Properties sind praktisch, solange man keine besonderen Regeln beim Setzen braucht. Sobald aber bestimmte Werte ungültig sind — z.B. ein negatives Semester oder ein leerer Name — reicht die Kurzschreibweise nicht mehr aus. In diesem Fall schreibt man `get` und `set` explizit aus und fügt die Prüfung direkt hinzu.

## Property mit Validierung

Wenn man Werte prüfen möchte, bevor sie gespeichert werden, schreibt man `get` und `set` explizit aus. Dabei muss man ein **eigenes privates Feld** deklarieren, in dem der Wert tatsächlich gespeichert wird — bei Auto-Properties erledigt der Compiler das unsichtbar im Hintergrund, aber sobald man eigene Logik in `get` oder `set` braucht, muss man dieses Feld selbst anlegen:

```csharp
class Student
{
    private int semester;

    public int Semester
    {
        get => semester;
        set
        {
            if (value < 1 || value > 20)
                throw new ArgumentOutOfRangeException("Ungültiges Semester.");
            semester = value;
        }
    }
}
```

Übung: Erstelle eine Klasse `Produkt` mit Properties `Name` (string, nur lesbar nach Erstellung) und `Preis` (double, muss positiv sein). Verwende ein Auto-Property für `Name` und ein Property mit Validierung für `Preis`.
{: .notice--info}

## Weitere Quellen

- [Properties – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/properties)
- [Auto-implementierte Properties – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties)
- [Kapselung (OOP-Konzept) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/object-oriented/)
