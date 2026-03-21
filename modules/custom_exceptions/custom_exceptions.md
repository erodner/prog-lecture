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

Eine eigene Exception erbt von `Exception` (oder einer spezifischeren Basisklasse):

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

## Verwenden der eigenen Exception

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

- Name endet immer auf `Exception`
- Erbt von `Exception` oder einer passenden Basisklasse
- Enthält mindestens den Konstruktor mit `string message`
