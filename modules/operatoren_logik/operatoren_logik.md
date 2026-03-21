---
title: "Vergleichs- und Logikoperatoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Ist der Benutzer alt genug? Hat er ein gültiges Ticket? Liegt die Temperatur in einem bestimmten Bereich? Solche Fragen beantwortet man im Code mit Vergleichs- und Logikoperatoren – sie sind das Fundament jeder Entscheidungslogik.

## Vergleichsoperatoren

Vergleiche geben immer einen `bool`-Wert (`true` oder `false`) zurück.

```csharp
int a = 10, b = 20;

Console.WriteLine(a == b);  // false – gleich
Console.WriteLine(a != b);  // true  – ungleich
Console.WriteLine(a <  b);  // true  – kleiner als
Console.WriteLine(a >  b);  // false – größer als
Console.WriteLine(a <= b);  // true  – kleiner oder gleich
Console.WriteLine(a >= b);  // false – größer oder gleich
```

## Logikoperatoren

```csharp
bool sonnig = true;
bool warm   = false;

Console.WriteLine(sonnig && warm);   // false – UND: beide müssen true sein
Console.WriteLine(sonnig || warm);   // true  – ODER: mindestens einer muss true sein
Console.WriteLine(!sonnig);          // false – NICHT: kehrt den Wert um
```

## Kombinierte Bedingungen

```csharp
int alter = 20;
bool hatAusweis = true;

if (alter >= 18 && hatAusweis)
    Console.WriteLine("Zutritt erlaubt.");

int note = 2;
if (note == 1 || note == 2)
    Console.WriteLine("Bestanden mit Auszeichnung.");
```

## Kurzschlussauswertung

C# wertet `&&` und `||` von links nach rechts aus und bricht frühzeitig ab:

```csharp
// Wenn alter < 18 → false, der zweite Teil wird gar nicht ausgewertet
if (alter >= 18 && DatenbankPrüfung())
{ ... }
```

Dies ist wichtig, wenn der zweite Ausdruck aufwändig oder fehleranfällig ist.

Übung: Schreibe eine Bedingung, die prüft ob eine Zahl zwischen 1 und 100 liegt (inklusive).
{: .notice--info}
