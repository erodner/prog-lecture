---
title: "Enumerationen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Was bedeutet `2` als Ampelfarbe? Oder `1` als Richtung? Solche „magischen Zahlen" machen Code schwer verständlich und fehleranfällig. Enumerationen lösen das Problem: sie ersetzen Zahlen durch sprechende Namen und machen Zustandsmodellierung im Code sofort lesbar.

## Was ist eine Enumeration?

Eine `enum` definiert eine Menge benannter Ganzzahlkonstanten. Sie machen Code lesbarer, weil magische Zahlen durch aussagekräftige Namen ersetzt werden.

```csharp
// Ohne enum – unleserlich:
int richtung = 2; // Was bedeutet 2?

// Mit enum – selbsterklärend:
enum Richtung { Nord, Ost, Süd, West }
Richtung richtung = Richtung.Süd;
```

## Deklaration

Eine `enum`-Deklaration listet die möglichen Werte auf. Intern wird jeder Wert als Ganzzahl gespeichert — standardmäßig beginnt der erste bei `0` und erhöht sich um `1`. Man kann die Werte aber auch explizit angeben:

```csharp
enum Wochentag { Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag }

enum Ampelfarbe
{
    Rot = 0,
    Gelb = 1,
    Grün = 2
}
```

## Verwendung

Enumerationen lassen sich in `if`-Abfragen und besonders gut in `switch`-Ausdrücken verwenden — der Compiler warnt sogar, wenn man einen möglichen Wert vergisst:

```csharp
Wochentag heute = Wochentag.Mittwoch;

if (heute == Wochentag.Samstag || heute == Wochentag.Sonntag)
    Console.WriteLine("Wochenende!");
else
    Console.WriteLine("Werktag.");

// switch mit enum:
string aktivität = heute switch
{
    Wochentag.Montag => "Wochenstart",
    Wochentag.Freitag => "Fast Wochenende!",
    Wochentag.Samstag or Wochentag.Sonntag => "Freizeit",
    _ => "Normaler Tag"
};
```

## Konvertierung

Da Enumerationen intern Ganzzahlen sind, kann man zwischen `enum` und `int` hin- und herkonvertieren. `ToString()` auf einem Enum-Wert liefert den Namen als String — nützlich für die Ausgabe:

```csharp
Ampelfarbe farbe = Ampelfarbe.Grün;
int wert = (int)farbe;         // 2
Console.WriteLine(farbe);     // "Grün"

Ampelfarbe zurück = (Ampelfarbe)1; // Gelb
```

## Flags-Enumerationen: mehrere Werte kombinieren

Normale Enumerationen modellieren genau einen Zustand. Manchmal braucht man aber eine Kombination: eine Datei kann gleichzeitig lesbar und schreibbar sein, ein Benutzer kann mehrere Rollen gleichzeitig haben. Dafür gibt es `[Flags]`.

Die Werte müssen **Zweierpotenzen** sein, damit jeder Wert genau ein Bit belegt und sich Kombinationen eindeutig darstellen lassen:

```csharp
[Flags]
enum Berechtigung
{
    Keine    = 0,
    Lesen    = 1,   // Bit 0: 001
    Schreiben = 2,  // Bit 1: 010
    Ausführen = 4,  // Bit 2: 100
    Alle     = Lesen | Schreiben | Ausführen  // 111 = 7
}
```

Kombinieren erfolgt mit `|` (bitweises ODER), prüfen mit `HasFlag`:

```csharp
Berechtigung nutzer = Berechtigung.Lesen | Berechtigung.Schreiben;

Console.WriteLine(nutzer);                              // Lesen, Schreiben
Console.WriteLine(nutzer.HasFlag(Berechtigung.Lesen));  // true
Console.WriteLine(nutzer.HasFlag(Berechtigung.Ausführen)); // false
Console.WriteLine((int)nutzer);                         // 3 (001 | 010 = 011)
```

Einzelne Flags lassen sich hinzufügen oder entfernen:

```csharp
// Ausführen hinzufügen:
nutzer |= Berechtigung.Ausführen;

// Schreiben entfernen:
nutzer &= ~Berechtigung.Schreiben;

Console.WriteLine(nutzer); // Lesen, Ausführen
```

Ohne `[Flags]` würde `Console.WriteLine` nur die Zahl ausgeben statt der lesbaren Kombination – das Attribut aktiviert die sprechende `ToString()`-Darstellung.
{: .notice--primary}

## Weitere Quellen

- [Enumerationstypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/enum)
- [Flags-Enumerationen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.flagsattribute)
