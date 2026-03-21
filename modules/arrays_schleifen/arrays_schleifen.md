---
title: "Arrays und Schleifen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Arrays und Schleifen gehören zusammen wie Topf und Deckel – kaum etwas ist nützlicher, als eine strukturierte Datenreihe systematisch zu durchlaufen, zu summieren oder auszuwerten. Die Wahl zwischen `for` und `foreach` hängt davon ab, ob man den Index benötigt.

Beim Durchlaufen eines Arrays zeigt sich **Mustererkennung** (*Pattern Recognition*) als Grundprinzip des Computational Thinking besonders deutlich: Ob man Temperaturen auswertet, Noten summiert oder Würfelergebnisse zählt – das zugrunde liegende Muster ist immer dasselbe. Wer dieses Muster einmal verstanden hat, kann es auf beliebige Probleme anwenden.

## Arrays und `for`

Wenn der Index benötigt wird, ist `for` die richtige Wahl:

```csharp
int[] temperaturen = { 18, 22, 17, 25, 21, 19, 24 };

// Alle Temperaturen ausgeben mit Tag-Nummer
for (int i = 0; i < temperaturen.Length; i++)
    Console.WriteLine($"Tag {i + 1}: {temperaturen[i]}°C");

// Array rückwärts ausgeben
for (int i = temperaturen.Length - 1; i >= 0; i--)
    Console.Write(temperaturen[i] + " ");
```

## Arrays und `foreach`

Wenn nur die Werte gebraucht werden (kein Index):

```csharp
double summe = 0;
foreach (int t in temperaturen)
    summe += t;

Console.WriteLine($"Durchschnitt: {summe / temperaturen.Length:F1}°C");
```

## Array befüllen mit Schleife

```csharp
// Quadratzahlen generieren
int[] quadrate = new int[10];
for (int i = 0; i < quadrate.Length; i++)
    quadrate[i] = (i + 1) * (i + 1);

// Ausgabe: 1 4 9 16 25 36 49 64 81 100
foreach (int q in quadrate)
    Console.Write(q + " ");
```

## Häufigster Wert

Bevor man Code schreibt, lohnt es sich, den Algorithmus als Pseudocode zu formulieren:

```
häufigsterWert(würfe):
    lege ein Zählarray für alle möglichen Werte an
    für jeden Wurf: erhöhe den Zähler für diesen Wert
    durchsuche das Zählarray nach dem Maximum
    gib den Index des Maximums zurück
```

In C#:

```csharp
int[] würfe = { 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5 };
int[] häufigkeit = new int[10]; // Index = Augenzahl

foreach (int w in würfe)
    häufigkeit[w]++;

int max = 0, häufigstes = 0;
for (int i = 1; i < häufigkeit.Length; i++)
    if (häufigkeit[i] > max)
    {
        max = häufigkeit[i];
        häufigstes = i;
    }

Console.WriteLine($"Häufigster Wert: {häufigstes} (kam {max}× vor)");
```

Übung: Generiere ein Array mit 20 Zufallszahlen zwischen 1 und 6 (`Random.Shared.Next(1, 7)`) und zähle, wie oft jede Zahl vorkommt.
{: .notice--info}
