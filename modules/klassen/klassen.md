---
title: "Klassen und Objekte"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

In der realen Welt gibt es viele Dinge, die ähnlich aufgebaut sind – jeder Student hat einen Namen, eine Matrikelnummer und einen Notendurchschnitt. Klassen erlauben es, genau solche Konzepte im Code abzubilden: als Bauplan für Objekte, die Daten und Verhalten in sich vereinen.

## Was ist eine Klasse?

Eine **Klasse** ist ein Bauplan für Objekte. Sie beschreibt, welche **Daten** (Felder/Properties) und welches **Verhalten** (Methoden) Objekte dieses Typs haben.

```csharp
class Student
{
    public string Name { get; set; }
    public int Matrikelnummer { get; set; }
    public double Gpa { get; set; }

    public Student(string name, int matrikelnummer)
    {
        Name = name;
        Matrikelnummer = matrikelnummer;
        Gpa = 0.0;
    }

    public void Vorstellen()
    {
        Console.WriteLine($"Ich bin {Name}, Matrikel-Nr. {Matrikelnummer}.");
    }
}
```

## Was ist ein Objekt?

Ein **Objekt** ist eine konkrete Instanz einer Klasse, erstellt mit `new`:

```csharp
Student s1 = new Student("Anna", 12345);
Student s2 = new Student("Ben", 67890);

s1.Vorstellen(); // "Ich bin Anna, Matrikel-Nr. 12345."
s2.Gpa = 1.7;
```

## Klasse vs. Objekt

| Begriff | Analogie | C# |
| :--- | :--- | :--- |
| Klasse | Bauplan eines Hauses | `class Student { ... }` |
| Objekt | Ein konkretes Haus | `new Student(...)` |

Übung: Modelliere eine Klasse `Konto` mit Kontonummer, Inhaber und Kontostand. Füge Methoden `Einzahlen` und `Auszahlen` hinzu.
{: .notice--info}
