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

```csharp
enum Wochentag { Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag }

enum Ampelfarbe
{
    Rot = 0,
    Gelb = 1,
    Grün = 2
}
```

Standardmäßig beginnt der erste Wert bei 0 und erhöht sich um 1.

## Verwendung

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

```csharp
Ampelfarbe farbe = Ampelfarbe.Grün;
int wert = (int)farbe;         // 2
Console.WriteLine(farbe);     // "Grün"

Ampelfarbe zurück = (Ampelfarbe)1; // Gelb
```

## Flags (mehrere Werte kombinieren)

```csharp
[Flags]
enum Berechtigung { Lesen = 1, Schreiben = 2, Ausführen = 4 }

Berechtigung b = Berechtigung.Lesen | Berechtigung.Schreiben;
Console.WriteLine(b.HasFlag(Berechtigung.Lesen));    // true
Console.WriteLine(b.HasFlag(Berechtigung.Ausführen)); // false
```
