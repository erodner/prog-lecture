---
title: "Methoden definieren und aufrufen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Stell dir vor, du bräuchtest dieselbe Berechnung an zehn verschiedenen Stellen in deinem Programm – und dann ändert sich die Formel. Ohne Methoden müsstest du zehn Stellen anpassen. Mit einer Methode änderst du einmal und alles funktioniert. Das ist der Kern von wiederverwendbarem Code.

Methoden sind direkter Ausdruck von **Zerlegung** (*Decomposition*) als einem der Grundprinzipien des Computational Thinking: Ein komplexes Problem wird in kleinere, benennbare Teilprobleme aufgeteilt – jede Methode löst genau eines davon. Wer lernt, Aufgaben sauber in Methoden zu zerlegen, denkt bereits wie eine Softwareentwicklerin.

## Warum Methoden?

Ohne Methoden wiederholt man denselben Code an vielen Stellen. Methoden erlauben es, Code zu **benennen**, **wiederzuverwenden** und **zu testen**.

```csharp
// Ohne Methode: Code-Duplizierung
double fl1 = 5.0 * 3.0;
double fl2 = 4.0 * 7.0;

// Mit Methode: klar und wiederverwendbar
double Fläche(double breite, double höhe) => breite * höhe;
double fl1 = Fläche(5.0, 3.0);
double fl2 = Fläche(4.0, 7.0);
```

## Aufbau einer Methode

```csharp
Rückgabetyp MethodenName(Typ param1, Typ param2)
{
    // Methodenrumpf
    return ergebnis;
}
```

## Methoden mit Rückgabewert

```csharp
static double Kreisfläche(double radius)
{
    return Math.PI * radius * radius;
}

// Aufruf:
double f = Kreisfläche(5.0);
Console.WriteLine($"Fläche: {f:F2}"); // 78,54
```

## Methoden ohne Rückgabewert (`void`)

```csharp
static void BegrüßeNutzer(string name)
{
    Console.WriteLine($"Hallo, {name}!");
    Console.WriteLine("Willkommen im System.");
}

// Aufruf:
BegrüßeNutzer("Anna");
```

## Mehrere Parameter

```csharp
static string NoteBeschreibung(int punkte)
{
    if (punkte >= 90) return "sehr gut";
    if (punkte >= 75) return "gut";
    if (punkte >= 60) return "befriedigend";
    if (punkte >= 50) return "ausreichend";
    return "nicht bestanden";
}

Console.WriteLine(NoteBeschreibung(82)); // gut
```

Übung: Schreibe Methoden `Maximum(int a, int b)`, `Minimum(int a, int b)` und `Durchschnitt(double[] werte)`.
{: .notice--info}
