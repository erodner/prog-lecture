---
title: "Wann sind Exceptions sinnvoll?"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Exceptions sind ein mächtiges Werkzeug – aber wie jedes Werkzeug kann man sie falsch einsetzen. Wer sie für normale Programmlogik nutzt, schreibt langsamen und schwer lesbaren Code. Wer sie komplett ignoriert, riskiert stillschweigende Fehler. Die Kunst liegt im richtigen Einsatz.

## Exceptions für außergewöhnliche Situationen

Exceptions sind nicht für den normalen Programmfluss gedacht – sie signalisieren **unerwartete oder fehlerhafte Zustände**.

```csharp
// Falsch: Exception für normale Logik missbrauchen
try {
    int wert = wörterbuch["schlüssel"];
} catch (KeyNotFoundException) {
    // "nicht vorhanden" ist keine Ausnahme!
}

// Richtig: Prüfen, bevor zugegriffen wird
if (wörterbuch.TryGetValue("schlüssel", out int wert))
    Console.WriteLine(wert);
```

Das `TryGetValue`/`TryParse`-Muster kennen wir bereits — es ist die saubere Alternative zu einer Exception bei erwartbaren Situationen.

## Exceptions nicht schlucken

Genauso wichtig wie die Frage, *wann* man Exceptions wirft, ist die Frage, wie man mit gefangenen Exceptions umgeht. Ein leerer `catch`-Block ist einer der gefährlichsten Fehler, die man machen kann — er versteckt Probleme, statt sie zu lösen. Das Programm läuft scheinbar weiter, aber intern ist etwas schiefgegangen:

```csharp
// Sehr schlechte Praxis – Fehler werden versteckt:
try {
    RiskanteOperation();
} catch (Exception) {
    // nichts tun
}

// Besser: mindestens loggen oder weiterwerfen
try {
    RiskanteOperation();
} catch (Exception ex) {
    Console.Error.WriteLine($"Fehler: {ex.Message}");
    throw; // Exception weiterwerfen
}
```

## Exception-Typen passend wählen

```csharp
static void SetzeAlter(int alter)
{
    if (alter < 0 || alter > 150)
        throw new ArgumentOutOfRangeException(nameof(alter), "Alter muss zwischen 0 und 150 liegen.");
}

static void Verarbeite(string eingabe)
{
    if (eingabe == null)
        throw new ArgumentNullException(nameof(eingabe));
    if (string.IsNullOrWhiteSpace(eingabe))
        throw new ArgumentException("Eingabe darf nicht leer sein.", nameof(eingabe));
}
```

Die beiden vorherigen Abschnitte zeigen: Sowohl die Wahl des Exception-Typs als auch die Validierung von Methodenparametern tragen dazu bei, dass Fehler schnell lokalisiert werden können. Die folgende Tabelle fasst die wichtigsten Empfehlungen zusammen.

## Richtlinien auf einen Blick

| Situation | Empfehlung |
| :--- | :--- |
| Ungültige Parameter | `ArgumentException` / `ArgumentNullException` |
| Erwarteter Fehlerfall (z.B. Schlüssel fehlt) | Rückgabewert / `TryParse`-Muster |
| Unerwarteter Zustand | Eigene Exception |
| Exception abfangen ohne Handlung | Nie – mindestens loggen |
| Mehrere Exceptions: Reihenfolge | Spezifisch vor allgemein |

## `using`-Statement statt `try-finally`

Zum Abschluss noch ein Muster, das in der Praxis sehr häufig vorkommt und eng mit der Ausnahmebehandlung zusammenhängt: das automatische Aufräumen von Ressourcen. Im Modul zu `try/catch/finally` haben wir gesehen, wie `finally` zum Aufräumen genutzt wird. Das `using`-Statement ist eine elegantere Alternative für Objekte, die nach Verwendung geschlossen werden müssen (z.B. Dateien, Datenbankverbindungen). Es ruft automatisch `Dispose()` auf — auch bei Exceptions:

```csharp
// Statt try-finally:
using (var reader = new StreamReader("datei.txt"))
{
    string inhalt = reader.ReadToEnd();
} // reader.Dispose() wird automatisch aufgerufen – auch bei Exception
```

## Weitere Quellen

- [Ausnahmen entwerfen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/design-guidelines/exceptions)
- [Best Practices für Ausnahmen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/exceptions/best-practices-for-exceptions)
