---
title: "Konstruktoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Wenn ein neues Objekt entsteht, soll es von Anfang an in einem gültigen Zustand sein – ein Bankkonto ohne Kontonummer oder ein Student ohne Namen macht wenig Sinn. Konstruktoren stellen genau das sicher: sie werden automatisch beim Erzeugen eines Objekts aufgerufen und können dabei alle nötigen Werte einfordern.

## Was ist ein Konstruktor?

Ein Konstruktor ist eine spezielle Methode, die beim Erzeugen eines Objekts mit `new` automatisch aufgerufen wird. Er hat **denselben Namen wie die Klasse** und **keinen Rückgabetyp**.

```csharp
class Rechteck
{
    double breite;
    double höhe;

    // Konstruktor
    public Rechteck(double breite, double höhe)
    {
        this.breite = breite;
        this.höhe = höhe;
    }

    public double Fläche() => breite * höhe;
}

// Objekt erzeugen → Konstruktor wird aufgerufen
Rechteck r = new Rechteck(5.0, 3.0);
Console.WriteLine(r.Fläche()); // 15
```

## Standardkonstruktor

Wenn kein Konstruktor definiert wird, generiert C# automatisch einen parameterlosen Standardkonstruktor. Sobald ein eigener Konstruktor definiert wird, entfällt dieser.

## Mehrere Konstruktoren (Überladung)

```csharp
class Kreis
{
    double radius;

    public Kreis()               { radius = 1.0; }   // Einheitskreis
    public Kreis(double radius)  { this.radius = radius; }

    public double Umfang() => 2 * Math.PI * radius;
}

var k1 = new Kreis();       // radius = 1
var k2 = new Kreis(5.0);    // radius = 5
```

## Konstruktor mit Validierung

```csharp
class Student
{
    public string Name { get; }
    public int Matrikelnummer { get; }

    public Student(string name, int matrikelnummer)
    {
        if (string.IsNullOrWhiteSpace(name))
            throw new ArgumentException("Name darf nicht leer sein.");
        if (matrikelnummer <= 0)
            throw new ArgumentException("Ungültige Matrikelnummer.");

        Name = name;
        Matrikelnummer = matrikelnummer;
    }
}
```

Übung: Erweitere die Klasse `Rechteck` um einen Konstruktor, der nur eine Seitenlänge nimmt (für ein Quadrat).
{: .notice--info}
