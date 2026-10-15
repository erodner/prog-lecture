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

Ohne `try-catch` würde das Programm bei ungültiger Eingabe mit einer `FormatException` abstürzen. Der `catch`-Block fängt genau diesen Fehlertyp ab und zeigt stattdessen eine freundliche Meldung. Das Programm läuft danach normal weiter.

## Mehrere catch-Blöcke

Im einfachen Beispiel haben wir nur eine Art von Fehler behandelt. In der Praxis können aber innerhalb eines `try`-Blocks verschiedene Exceptions auftreten, die jeweils eine andere Reaktion erfordern. Wenn verschiedene Fehler auftreten können, die unterschiedlich behandelt werden sollen, kann man mehrere `catch`-Blöcke hintereinander schreiben. Der erste passende Block wird ausgeführt:

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

## `finally` — immer ausführen

`finally` wird ausgeführt, **egal ob eine Exception auftrat oder nicht**. Das ist besonders wichtig für Aufräumarbeiten — z.B. geöffnete Dateien schließen, Netzwerkverbindungen beenden oder temporäre Ressourcen freigeben:

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

Mit `try`, `catch` und `finally` können wir auf Fehler reagieren — aber manchmal muss unser eigener Code den Fehler *auslösen*. Exceptions werden nicht nur von der Laufzeitumgebung geworfen — man kann sie auch bewusst mit `throw` selbst auslösen. Das ist sinnvoll, wenn eine Methode ungültige Eingaben erkennt und den Aufrufer darauf aufmerksam machen will:

```csharp
static double Teilen(double a, double b)
{
    if (b == 0)
        throw new DivideByZeroException("Divisor darf nicht null sein.");
    return a / b;
}
```

Damit können wir endlich die Lücke schließen, die uns in Vorlesung 11 begegnet ist. Dort haben wir ungültige Werte mit `bool`-Rückgabewerten oder Konsolenmeldungen abgewiesen — beim [Semester-Property](/modules/properties/properties.md) und im [Student-Konstruktor](/modules/konstruktoren/konstruktoren.md) blieb uns sogar nur, einen Ersatzwert zu verwenden, weil Setter und Konstruktoren nichts zurückgeben können. Mit `throw` kann der Konstruktor die Erzeugung des Objekts jetzt tatsächlich abbrechen:

```csharp
public Student(string name, int matrikelnummer)
{
    if (string.IsNullOrWhiteSpace(name))
        throw new ArgumentException("Name darf nicht leer sein.");
    if (matrikelnummer <= 0)
        throw new ArgumentException("Ungültige Matrikelnummer.");

    Name = name;
    Matrikelnummer = matrikelnummer;
}
```

`new Student("", -5)` liefert nun kein Objekt mit Platzhalterwerten mehr — es entsteht überhaupt kein Objekt, und der Aufrufer *muss* reagieren, sonst bricht das Programm ab. Anders als ein `bool`-Rückgabewert lässt sich eine Exception nicht versehentlich ignorieren. Wann `bool` trotzdem die bessere Wahl ist, besprechen wir im Modul zum [Exception-Design](/modules/exceptions_design/exceptions_design.md).

Übung: Schreibe eine Methode, die sicher einen `int` aus einer Konsoleneingabe liest und bei ungültiger Eingabe immer wieder fragt (Schleife + try-catch).
{: .notice--info}

Wichtig: Wenn man eine gefangene Exception weiterwerfen will, sollte man `throw;` statt `throw ex;` verwenden. `throw;` behält den ursprünglichen Stack-Trace bei, während `throw ex;` ihn zurücksetzt — das erschwert die Fehlersuche erheblich.
{: .notice--warning}

## Weitere Quellen

- [Ausnahmebehandlungsanweisungen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/statements/exception-handling-statements)
- [Best Practices für Ausnahmen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/exceptions/best-practices-for-exceptions)
