---
title: "do-while-Schleife"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Ein Menü soll immer mindestens einmal angezeigt werden, bevor der Benutzer entscheiden kann ob er weitermacht. Genau für solche Fälle gibt es die `do-while`-Schleife: sie führt den Schleifenrumpf zuerst aus und prüft die Bedingung erst danach.

## Syntax

```csharp
do
{
    // wird mindestens einmal ausgeführt
} while (Bedingung);
```

Im Unterschied zur `while`-Schleife wird die Bedingung **nach** dem Schleifenrumpf geprüft – der Rumpf wird also **mindestens einmal** ausgeführt.

## Vergleich mit `while`

```csharp
int x = 10;

// while: Rumpf wird nie ausgeführt (Bedingung sofort false)
while (x < 5)
    Console.WriteLine(x);

// do-while: Rumpf wird einmal ausgeführt
do
    Console.WriteLine(x);
while (x < 5);
// Ausgabe: 10
```

## Typischer Einsatz: Menüschleife

```csharp
string auswahl;
do
{
    Console.WriteLine("1 - Spielen");
    Console.WriteLine("2 - Einstellungen");
    Console.WriteLine("0 - Beenden");
    Console.Write("Auswahl: ");
    auswahl = Console.ReadLine();

    // Auswahl verarbeiten...

} while (auswahl != "0");

Console.WriteLine("Auf Wiedersehen!");
```

Das Menü soll immer mindestens einmal angezeigt werden – `do-while` ist hier die natürliche Wahl.

Übung: Schreibe ein Ratespiel: Das Programm denkt sich eine Zahl zwischen 1 und 10. Der Spieler rät, bis er richtig liegt. Nutze `do-while`.
{: .notice--info}
