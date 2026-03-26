---
title: "Eigene Ausnahmen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Eine allgemeine `Exception` mit einem Freitext ist oft nicht aussagekräftig genug – wer sieht „Fehler aufgetreten", weiß nicht sofort was schiefgelaufen ist. Eigene Exception-Klassen geben Fehlern einen präzisen Namen und können zusätzliche Informationen transportieren, die beim Behandeln des Fehlers hilfreich sind.

## Warum eigene Exceptions?

Eigene Exception-Klassen machen Fehler **aussagekräftiger** und erlauben es, domänenspezifische Fehlerfälle klar zu benennen.

```csharp
// Allgemein – wenig aussagekräftig:
throw new Exception("Ungültiges Passwort");

// Spezifisch – selbsterklärend:
throw new UngültigesPasswortException("Passwort zu kurz: mindestens 8 Zeichen.");
```

## Eigene Exception erstellen

Eine eigene Exception erbt von `Exception` (oder einer spezifischeren Basisklasse). Der Konstruktor ruft mit `: base(message)` den Konstruktor der Basisklasse auf und übergibt die Fehlermeldung. Eigene Properties (wie `Fehlbetrag`) können zusätzliche Informationen transportieren:

```csharp
class KontoException : Exception
{
    public KontoException(string message) : base(message) { }
}

class UngedeckterBetragException : KontoException
{
    public double Fehlbetrag { get; }

    public UngedeckterBetragException(double fehlbetrag)
        : base($"Kontostand um {fehlbetrag:F2} € überschritten.")
    {
        Fehlbetrag = fehlbetrag;
    }
}
```

Man sieht hier auch eine eigene **Exception-Hierarchie**: `UngedeckterBetragException` erbt von `KontoException`, die wiederum von `Exception` erbt. So kann der Aufrufer entweder spezifisch `UngedeckterBetragException` fangen oder allgemeiner `KontoException` für alle kontobezogenen Fehler.

## Verwenden der eigenen Exception

Nachdem wir die Exception-Klassen definiert haben, wollen wir sie nun in einer realistischen Anwendung einsetzen. Die eigene Exception wird genau wie eine eingebaute mit `throw` geworfen und mit `catch` gefangen. Der Vorteil: Im `catch`-Block hat man Zugriff auf die spezifischen Properties — hier `Fehlbetrag`:

```csharp
class Konto
{
    public double Stand { get; private set; } = 0;

    public void Einzahlen(double betrag)
    {
        if (betrag <= 0)
            throw new ArgumentException("Betrag muss positiv sein.");
        Stand += betrag;
    }

    public void Abheben(double betrag)
    {
        if (betrag > Stand)
            throw new UngedeckterBetragException(betrag - Stand);
        Stand -= betrag;
    }
}

var konto = new Konto();
konto.Einzahlen(100);

try
{
    konto.Abheben(150);
}
catch (UngedeckterBetragException ex)
{
    Console.WriteLine($"Abhebung fehlgeschlagen: {ex.Message}");
    Console.WriteLine($"Fehlbetrag: {ex.Fehlbetrag:F2} €");
}
```

## Konventionen

Damit eigene Exceptions sich nahtlos in das .NET-Ökosystem einfügen und von anderen Entwicklern sofort verstanden werden, haben sich einige Konventionen etabliert:

- Name endet immer auf `Exception`
- Erbt von `Exception` oder einer passenden Basisklasse
- Enthält mindestens den Konstruktor mit `string message`

## Weitere Quellen

- [Eigene Ausnahmen erstellen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/exceptions/creating-and-throwing-exceptions)
- [Ausnahmen entwerfen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/design-guidelines/exceptions)
