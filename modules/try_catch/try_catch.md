---
title: "try / catch / finally"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Eine Exception auftreten lassen und das Programm abstürzen zu sehen ist das eine – aber ein robustes Programm reagiert darauf sinnvoll, ohne zu crashen. Mit `try`, `catch` und `finally` umschließt man riskanten Code und gibt dem Programm die Chance, Fehler kontrolliert zu behandeln.

## Grundstruktur

```csharp
try
{
    // Code, der eine Exception werfen könnte
}
catch (ExceptionTyp ex)
{
    // wird ausgeführt, wenn die Exception auftritt
}
finally
{
    // wird IMMER ausgeführt – mit oder ohne Exception
}
```

## Einfaches Beispiel

```csharp
Console.Write("Zahl eingeben: ");
try
{
    int zahl = int.Parse(Console.ReadLine());
    Console.WriteLine($"Das Doppelte: {zahl * 2}");
}
catch (FormatException)
{
    Console.WriteLine("Fehler: Bitte eine gültige Zahl eingeben.");
}
```

## Mehrere catch-Blöcke

```csharp
try
{
    int[] arr = new int[3];
    string eingabe = Console.ReadLine();
    int idx = int.Parse(eingabe);
    Console.WriteLine(arr[idx]);
}
catch (FormatException)
{
    Console.WriteLine("Kein gültiger Index.");
}
catch (IndexOutOfRangeException)
{
    Console.WriteLine("Index außerhalb des Bereichs.");
}
catch (Exception ex)  // Auffangbecken für alle anderen Exceptions
{
    Console.WriteLine($"Unerwarteter Fehler: {ex.Message}");
}
```

Spezifischere Exceptions immer vor allgemeinen platzieren.
{: .notice--primary}

## `finally` – immer ausführen

`finally` wird ausgeführt, egal ob eine Exception auftrat oder nicht. Typisch für Aufräumarbeiten:

```csharp
StreamReader reader = null;
try
{
    reader = new StreamReader("daten.txt");
    string inhalt = reader.ReadToEnd();
    Console.WriteLine(inhalt);
}
catch (FileNotFoundException)
{
    Console.WriteLine("Datei nicht gefunden.");
}
finally
{
    reader?.Close(); // immer schließen, auch bei Fehler
}
```

## Exception selbst werfen

```csharp
static double Teilen(double a, double b)
{
    if (b == 0)
        throw new DivideByZeroException("Divisor darf nicht null sein.");
    return a / b;
}
```

Übung: Schreibe eine Methode, die sicher einen `int` aus einer Konsoleneingabe liest und bei ungültiger Eingabe immer wieder fragt (Schleife + try-catch).
{: .notice--info}
