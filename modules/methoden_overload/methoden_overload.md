---
title: "Methodenüberladung"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

`Console.WriteLine` kann eine Zahl, einen Text oder einen booleschen Wert ausgeben – immer mit demselben Namen. Dieses Prinzip heißt Überladung: mehrere Methoden teilen sich einen Namen, unterscheiden sich aber in ihren Parametern. C# wählt beim Aufruf automatisch die passende Version aus.

## Was ist Überladung?

Mehrere Methoden dürfen denselben Namen haben, wenn sie sich in **Anzahl oder Typ der Parameter** unterscheiden. Der Compiler wählt automatisch die passende Version.

```csharp
static double Fläche(double seite)                    // Quadrat
    => seite * seite;

static double Fläche(double breite, double höhe)      // Rechteck
    => breite * höhe;

static double Fläche(double radius, bool istKreis)    // Kreis
    => Math.PI * radius * radius;

// Aufrufe:
Console.WriteLine(Fläche(4.0));          // 16
Console.WriteLine(Fläche(3.0, 5.0));     // 15
Console.WriteLine(Fläche(3.0, true));    // 28,27
```

## Beispiel aus der BCL

Die Methode `Console.WriteLine` ist selbst massiv überladen:

```csharp
Console.WriteLine(42);        // int-Version
Console.WriteLine(3.14);      // double-Version
Console.WriteLine("Hallo");   // string-Version
Console.WriteLine(true);      // bool-Version
```

## Optionale Parameter als Alternative

Statt Überladung kann man auch Standardwerte verwenden:

```csharp
static void Begrüße(string name, string anrede = "Hallo")
{
    Console.WriteLine($"{anrede}, {name}!");
}

Begrüße("Anna");              // Hallo, Anna!
Begrüße("Prof. Müller", "Guten Tag"); // Guten Tag, Prof. Müller!
```

## Wann welches?

- **Überladung:** wenn sich die Parameter strukturell unterscheiden
- **Optionale Parameter:** wenn ein Parameter häufig denselben Standardwert hat
