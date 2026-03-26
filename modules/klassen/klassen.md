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

Das ist **Abstraktion** (*Abstraction*) in Reinform – eines der vier Grundprinzipien des Computational Thinking. Wer eine Klasse `Student` definiert, blendet alle Details der Implementierung aus und arbeitet stattdessen mit einem klaren, benannten Konzept. Der Rest des Programms muss nicht wissen, wie ein Student intern gespeichert ist – nur was er kann und was er hat.

## Was ist eine Klasse?

Eine **Klasse** ist ein Bauplan für Objekte. Sie beschreibt, welche **Daten** (Felder/Properties) und welches **Verhalten** (Methoden) Objekte dieses Typs haben.

```csharp
class Student
{
    public string Name { get; set; }
    public int Matrikelnummer { get; set; }
    public double Note { get; set; }

    public Student(string name, int matrikelnummer)
    {
        Name = name;
        Matrikelnummer = matrikelnummer;
        Note = 0.0;
    }

    public void Vorstellen()
    {
        Console.WriteLine($"Ich bin {Name}, Matrikel-Nr. {Matrikelnummer}.");
    }
}
```

Der Bauplan allein erzeugt noch nichts — er beschreibt nur die Struktur. Um tatsächlich mit konkreten Daten zu arbeiten, brauchen wir **Objekte**.

## Was ist ein Objekt?

Ein **Objekt** ist eine konkrete Instanz einer Klasse, erstellt mit `new`. Aus einem Bauplan können beliebig viele Objekte entstehen — jedes mit eigenen Daten, aber demselben Verhalten:

```csharp
Student s1 = new Student("Anna", 12345);
Student s2 = new Student("Ben", 67890);

s1.Vorstellen(); // "Ich bin Anna, Matrikel-Nr. 12345."
s2.Note = 1.7;
```

`s1` und `s2` sind zwei verschiedene Objekte mit unterschiedlichen Daten (Name, Matrikelnummer), aber beide haben dieselben Methoden und Properties, weil sie aus derselben Klasse stammen. Das kennen wir von Referenztypen: `s1` und `s2` sind Referenzen, die jeweils auf ein eigenes Objekt im Speicher zeigen.

Wichtig ist der Unterschied zwischen **Identität** und **Gleichheit**: Auch wenn zwei Studenten zufällig denselben Namen hätten, wären `s1` und `s2` trotzdem zwei unterschiedliche Objekte — sie liegen an verschiedenen Stellen im Speicher. Identität (`s1 == s2`) prüft bei Klassen standardmäßig, ob beide Referenzen auf **dasselbe** Objekt zeigen, nicht ob die Daten übereinstimmen.

Eine Klasse existiert nur einmal im Code. Objekte können zur Laufzeit beliebig viele erzeugt werden. Bisher haben wir mit eingebauten Typen wie `int`, `string` und `int[]` gearbeitet — Klassen erlauben es uns, **eigene Typen** zu definieren, die genau zu unserem Problem passen.

Übung: Modelliere eine Klasse `Konto` mit Kontonummer, Inhaber und Kontostand. Füge Methoden `Einzahlen` und `Auszahlen` hinzu.
{: .notice--info}

## Weitere Quellen

- [Klassen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/classes)
- [Objektorientierung in C# – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/object-oriented/)
- [OOP-Konzepte – C# Corner](https://www.c-sharpcorner.com/UploadFile/mkaaborern/object-oriented-programming-concepts-in-C-Sharp/)
