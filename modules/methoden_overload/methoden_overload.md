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

## Ein Beispiel: Umsatzsteuer berechnen

Stellen wir uns vor, wir wollen die Umsatzsteuer berechnen. Je nach Situation hat man unterschiedliche Ausgangsdaten: mal nur einen einzelnen Betrag, mal eine Liste von Einzelpositionen. Ohne Überladung bräuchte man für jede Variante einen eigenen Methodennamen:

```csharp
static double UmsatzsteuerVonBetrag(double netto) { ... }
static double UmsatzsteuerVonPositionen(double[] positionen) { ... }
```

Das funktioniert, aber der Aufrufer muss sich mehrere Namen merken — obwohl die Absicht immer dieselbe ist: Umsatzsteuer berechnen.

## Überladung: ein Name, mehrere Signaturen

Mit Überladung können beide Varianten `Umsatzsteuer` heißen. Der Compiler entscheidet **zur Compilierzeit** anhand der übergebenen Argumente, welche Version gemeint ist:

```csharp
// Variante 1: Umsatzsteuer aus einem einzelnen Nettobetrag
static double Umsatzsteuer(double netto)
{
    return netto * 0.19;
}

// Variante 2: Umsatzsteuer aus mehreren Einzelpositionen
static double Umsatzsteuer(double[] positionen)
{
    double summe = 0;
    foreach (double pos in positionen)
        summe += pos;
    return summe * 0.19;
}
```

```csharp
Console.WriteLine(Umsatzsteuer(100.0));                    // 19
Console.WriteLine(Umsatzsteuer(new double[] { 50, 30, 20 })); // 19
```

Der Aufrufer schreibt einfach `Umsatzsteuer` — der Compiler wählt anhand des Argumenttyps (`double` vs. `double[]`) die passende Variante. Beide Aufrufe liefern hier dasselbe Ergebnis, aber über unterschiedliche Wege.

Die Regel: Zwei Methoden dürfen denselben Namen haben, wenn sie sich in **Anzahl oder Typ der Parameter** unterscheiden. Der Rückgabetyp allein reicht nicht — zwei Methoden mit identischen Parametern aber unterschiedlichem Rückgabetyp sind ein Compilerfehler.
{: .notice--warning}

## Überladung in der BCL

Überladung ist kein exotisches Feature — man nutzt sie ständig, ohne es zu merken. `Console.WriteLine` selbst hat über ein Dutzend Überladungen in der .NET-Klassenbibliothek (BCL):

```csharp
Console.WriteLine(42);       // void WriteLine(int value)
Console.WriteLine(3.14);     // void WriteLine(double value)
Console.WriteLine("Hallo");  // void WriteLine(string value)
Console.WriteLine(true);     // void WriteLine(bool value)
```

Dasselbe gilt für `Math.Max`, `Math.Min`, `Math.Abs` und viele weitere — jeweils in Varianten für `int`, `double`, `float` usw.

## Optionale Parameter (Standardwerte)

Nicht immer braucht man eine eigene Überladung. Unsere `Umsatzsteuer`-Methode verwendet fest 19 % — aber was, wenn man auch den ermäßigten Satz von 7 % unterstützen will? Man könnte eine dritte Überladung schreiben, aber eleganter ist ein **optionaler Parameter** mit Standardwert:

```csharp
static double Umsatzsteuer(double netto, double satz = 0.19)
{
    return netto * satz;
}

Console.WriteLine(Umsatzsteuer(100.0));        // 19   (Standardsatz 19 %)
Console.WriteLine(Umsatzsteuer(100.0, 0.07));  // 7    (ermäßigter Satz 7 %)
```

Der Standardwert wird direkt in der Signatur mit `=` angegeben. Der Compiler setzt den Standardwert automatisch ein, wenn der Aufrufer das Argument weglässt. So muss man nicht für jeden Sonderwert eine eigene Überladung schreiben.

Optionale Parameter müssen immer **am Ende** der Parameterliste stehen — ein Parameter mit Standardwert darf nicht vor einem ohne stehen. Außerdem müssen Standardwerte **Kompilierzeitkonstanten** sein (also Zahlenliterale, Strings oder `null` — keine Methodenaufrufe oder berechnete Werte).
{: .notice--primary}

Natürlich lassen sich Überladung und optionale Parameter auch kombinieren. So entsteht eine flexible Schnittstelle, die den häufigsten Fall einfach hält und Sonderfälle trotzdem unterstützt:

```csharp
// Einzelbetrag mit optionalem Steuersatz
static double Umsatzsteuer(double netto, double satz = 0.19)
{
    return netto * satz;
}

// Mehrere Positionen mit optionalem Steuersatz
static double Umsatzsteuer(double[] positionen, double satz = 0.19)
{
    double summe = 0;
    foreach (double pos in positionen)
        summe += pos;
    return summe * satz;
}
```

```csharp
// Standardsatz:
Console.WriteLine(Umsatzsteuer(200.0));                          // 38
Console.WriteLine(Umsatzsteuer(new double[] { 100, 50, 50 }));   // 38

// Ermäßigter Satz:
Console.WriteLine(Umsatzsteuer(200.0, 0.07));                        // 14
Console.WriteLine(Umsatzsteuer(new double[] { 100, 50, 50 }, 0.07)); // 14
```

Der Aufrufer hat vier Möglichkeiten — alle heißen `Umsatzsteuer`, und der Compiler findet die richtige Variante anhand der Argumente.

Übung: Schreibe eine Methode `Rabatt(double preis)`, die 10 % Rabatt berechnet, und eine Überladung `Rabatt(double preis, double prozent)` mit wählbarem Rabattsatz. Überlege dann, ob sich das Problem eleganter mit einem einzigen optionalen Parameter lösen lässt.
{: .notice--info}

## Weitere Quellen

- [Methodenüberladung – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/design-guidelines/member-overloading)
- [Optionale und benannte Argumente – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
