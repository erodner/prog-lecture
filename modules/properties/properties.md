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

## Properties (Eigenschaften)

Properties bieten kontrollierten Zugriff auf interne Daten über `get` und `set`:

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

## Auto-Properties (Kurzschreibweise)

Wenn keine besondere Logik nötig ist:

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

## Property mit Validierung

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
