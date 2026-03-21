---
title: "Methoden in Klassen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Objekte haben nicht nur Daten – sie können auch etwas tun. Instanzmethoden beschreiben das Verhalten eines Objekts und haben dabei direkten Zugriff auf seine eigenen Felder und Properties. Statische Methoden gehören hingegen zur Klasse selbst, nicht zu einer konkreten Instanz.

## Instanzmethoden vs. statische Methoden

**Instanzmethoden** arbeiten auf einem konkreten Objekt – sie haben Zugriff auf die Felder und Properties der Instanz.

**Statische Methoden** gehören zur Klasse, nicht zu einem Objekt. Sie können nicht auf Instanzfelder zugreifen.

```csharp
class Konto
{
    public double Stand { get; private set; }

    // Instanzmethode – arbeitet auf diesem Objekt
    public void Einzahlen(double betrag) => Stand += betrag;
    public void Abheben(double betrag)
    {
        if (betrag > Stand)
            throw new InvalidOperationException("Nicht genug Guthaben.");
        Stand -= betrag;
    }

    // Statische Methode – kein Zugriff auf Stand nötig
    public static double EuroInDollar(double euro) => euro * 1.08;
}

var k = new Konto();
k.Einzahlen(500);
k.Abheben(200);
Console.WriteLine(k.Stand); // 300

Console.WriteLine(Konto.EuroInDollar(100)); // 108
```

## Methoden, die andere Methoden aufrufen

```csharp
class Bestellung
{
    List<double> positionen = new List<double>();

    public void HinzufügenPosition(double preis) => positionen.Add(preis);

    private double Nettobetrag() => positionen.Sum();
    private double Mehrwertsteuer() => Nettobetrag() * 0.19;

    public double Gesamtbetrag() => Nettobetrag() + Mehrwertsteuer();

    public void Ausgeben()
    {
        Console.WriteLine($"Netto:  {Nettobetrag(),8:F2} €");
        Console.WriteLine($"MwSt.:  {Mehrwertsteuer(),8:F2} €");
        Console.WriteLine($"Gesamt: {Gesamtbetrag(),8:F2} €");
    }
}
```

## `ToString` überschreiben

```csharp
class Punkt
{
    public double X { get; set; }
    public double Y { get; set; }

    public override string ToString() => $"({X}, {Y})";
}

var p = new Punkt { X = 3, Y = 4 };
Console.WriteLine(p); // (3, 4)
```
