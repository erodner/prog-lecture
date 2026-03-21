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

`Console.WriteLine` kann eine ganze Zahl, eine Kommazahl, einen Text oder einen booleschen Wert ausgeben – immer mit demselben Methodennamen. Wäre das nicht möglich, müsste man `WriteLineInt`, `WriteLineDouble`, `WriteLineString` und so weiter aufrufen. Das wäre umständlich und schwer zu merken. Dieses Prinzip heißt **Überladung**: mehrere Methoden teilen sich einen Namen, unterscheiden sich aber in ihren Parametern.

## Das Problem ohne Überladung

Angenommen, man möchte die Fläche verschiedener Formen berechnen. Ohne Überladung bräuchte man unterschiedliche Namen:

```csharp
static double QuadratFläche(double seite)      => seite * seite;
static double RechteckFläche(double b, double h) => b * h;
static double KreisFläche(double radius)       => Math.PI * radius * radius;
```

Das funktioniert, aber der Aufrufer muss sich drei verschiedene Namen merken – obwohl die Absicht immer dieselbe ist: eine Fläche berechnen.

## Überladung: ein Name, mehrere Signaturen

Mit Überladung können alle drei Methoden `Fläche` heißen. Der Compiler entscheidet anhand der übergebenen Parameter, welche Version gemeint ist:

```csharp
static double Fläche(double seite)
{
    return seite * seite;
}

static double Fläche(double breite, double höhe)
{
    return breite * höhe;
}

static double Fläche(double radius, bool istKreis)
{
    return Math.PI * radius * radius;
}
```

```csharp
Console.WriteLine(Fläche(4.0));         // 16   – Quadrat (ein Parameter)
Console.WriteLine(Fläche(3.0, 5.0));    // 15   – Rechteck (zwei double)
Console.WriteLine(Fläche(3.0, true));   // 28,27 – Kreis (double + bool)
```

Zwei Methoden dürfen denselben Namen haben, wenn sie sich in **Anzahl oder Typ der Parameter** unterscheiden. Der Rückgabetyp allein reicht nicht – zwei Methoden mit identischen Parametern aber unterschiedlichem Rückgabetyp sind ein Compilerfehler.
{: .notice--warning}

## Überladung in der BCL

Die Methode `Console.WriteLine` selbst ist ein Paradebeispiel – sie hat über ein Dutzend Überladungen in der BCL:

```csharp
Console.WriteLine(42);       // void WriteLine(int value)
Console.WriteLine(3.14);     // void WriteLine(double value)
Console.WriteLine("Hallo");  // void WriteLine(string value)
Console.WriteLine(true);     // void WriteLine(bool value)
```

Dasselbe gilt für `Math.Max`, `Math.Min`, `string.IndexOf` und viele weitere.

## Optionale Parameter als Alternative

Statt mehrerer Überladungen kann man Parameter mit Standardwerten versehen. Der Aufrufer kann sie dann weglassen:

```csharp
static void Begrüße(string name, string anrede = "Hallo")
{
    Console.WriteLine($"{anrede}, {name}!");
}

Begrüße("Anna");                       // Hallo, Anna!
Begrüße("Prof. Müller", "Guten Tag"); // Guten Tag, Prof. Müller!
```

Optionale Parameter stehen immer am Ende der Parameterliste – ein Parameter mit Standardwert darf nicht vor einem ohne stehen.
{: .notice--primary}

**Überladung oder optionale Parameter?**
- Überladung passt, wenn sich Methoden strukturell unterscheiden (andere Parametertypen, andere Berechnungslogik).
- Optionale Parameter passen, wenn eine Methode häufig mit denselben Standardwerten aufgerufen wird und sich die Logik nicht ändert.

Übung: Schreibe eine überladene Methode `Beschreibe`, die entweder einen `int`, einen `double` oder einen `string` entgegennimmt und jeweils eine passende Beschreibung ausgibt (z.B. „Ganzzahl: 42", „Kommazahl: 3,14", „Text: Hallo").
{: .notice--info}
