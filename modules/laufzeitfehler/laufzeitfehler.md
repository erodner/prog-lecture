---
title: "Laufzeitfehler und Logikfehler"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Nicht alle Fehler lassen sich beim Kompilieren finden. Manche treten erst auf, wenn das Programm läuft — entweder als Absturz (Laufzeitfehler) oder als stilles Fehlverhalten (Logikfehler). Beide Kategorien erfordern andere Strategien als Compilerfehler.

## Laufzeitfehler (Exceptions)

Der Code ist syntaktisch korrekt und lässt sich kompilieren, aber beim Ausführen tritt ein Fehler auf. Dies passiert zum Beispiel wenn ein Nutzer Werte eingibt, welche nicht verarbeitet werden können. Wir werden noch viel später kennenlernen wie man solche Fehler auch innerhalb des Programmes abfangen kann. Aktuell wird die Ausführung des Programms immer abgebrochen, wenn ein Laufzeitfehler (engl. *exception*) auftritt.

```csharp
string eingabe = Console.ReadLine();
int zahl = int.Parse(eingabe); // Laufzeitfehler, wenn Benutzer "abc" eingibt
```

## Häufige Laufzeitfehler

### System.FormatException

```csharp
int zahl = int.Parse("abc");   // FormatException: "abc" ist keine Zahl
```

Ein String soll in eine Zahl konvertiert werden, hat aber kein gültiges Format. Lösung: `TryParse` verwenden.

### System.DivideByZeroException

```csharp
int a = 10, b = 0;
Console.WriteLine(a / b);   // DivideByZeroException
```

Division durch null bei ganzen Zahlen. Bei `double` liefert `10.0 / 0` dagegen `Infinity` — kein Fehler, aber auch selten gewollt.

### System.IndexOutOfRangeException

```csharp
int[] zahlen = new int[3];
Console.WriteLine(zahlen[5]);   // IndexOutOfRangeException
```

Zugriff auf einen Index, der nicht existiert. Arrays zählen ab 0 — ein Array der Länge 3 hat die Indizes 0, 1 und 2.

### System.NullReferenceException

```csharp
string text = null;
Console.WriteLine(text.Length);   // NullReferenceException
```

Ein Methodenaufruf oder Eigenschaftszugriff auf `null` — es gibt kein Objekt, auf dem man operieren könnte. Dieser Fehler ist einer der häufigsten überhaupt.

### System.OverflowException

```csharp
checked
{
    int x = int.MaxValue + 1;   // OverflowException
}
```

Ein Wert überschreitet den Bereich seines Typs. Ohne `checked` rollt der Wert still um — noch gefährlicher.

## Logikfehler

Logikfehler sind die tückischste Kategorie: Das Programm läuft ohne Absturz durch, liefert aber **falsche Ergebnisse**. Es gibt keine Fehlermeldung — man bemerkt das Problem erst, wenn man das Ergebnis prüft.

```csharp
// Durchschnitt von drei Zahlen – aber falsch!
int a = 10, b = 20, c = 30;
double durchschnitt = a + b + c / 3; // Fehler: Punkt vor Strich!
Console.WriteLine(durchschnitt);     // Gibt 40 aus, nicht 20

// Richtig:
double korrekt = (a + b + c) / 3.0;
```

Logikfehler sind **am schwierigsten zu finden**, weil das Programm nicht abstürzt, aber einfach Ergebnisse liefert, welche nicht den Anforderungen genügen. Hier helfen systematisches Testen und die Debugging-Strategien aus dem nächsten Modul.

Übung: Welche Fehlerart liegt vor? `int x = 10 / 0;` — Compilerfehler, Laufzeitfehler oder Logikfehler?
{: .notice--info}

## Weitere Quellen

- [Häufige Laufzeit-Exceptions – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/exceptions/)
- [Ausnahmen und Fehlerbehandlung – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/exceptions/)
- [Debugger-Einführung – Microsoft Learn](https://learn.microsoft.com/de-de/visualstudio/debugger/debugger-feature-tour)
