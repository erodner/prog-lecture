---
title: "Eindimensionale Arrays"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Fünf Noten, sieben Wochentage, hundert Messwerte – sobald man viele gleichartige Daten verwalten muss, stößt man mit einzelnen Variablen schnell an Grenzen. Arrays sind die einfachste Lösung: eine geordnete Folge von Werten desselben Typs, auf die man per Index zugreift.

## Was ist ein Array?

Ein Array ist eine **feste Folge gleichartiger Werte** unter einem gemeinsamen Namen. Im Gegensatz zu einzelnen Variablen lassen sich damit viele Werte strukturiert verwalten.

```
Index:  [0]  [1]  [2]  [3]  [4]
         ┌────┬────┬────┬────┬────┐
noten:   │ 2  │ 1  │ 3  │ 2  │ 4  │
         └────┴────┴────┴────┴────┘
```

## Deklaration und Initialisierung

```csharp
// Leer anlegen (Standardwerte: 0 für int)
int[] noten = new int[5];

// Direkt befüllen
int[] noten = { 2, 1, 3, 2, 4 };
string[] wochentage = { "Mo", "Di", "Mi", "Do", "Fr" };
```

## Zugriff über Index

Indizes starten bei **0**:

```csharp
Console.WriteLine(noten[0]);   // 2 – erstes Element
Console.WriteLine(noten[4]);   // 4 – letztes Element
Console.WriteLine(noten.Length); // 5 – Anzahl Elemente

noten[2] = 5;  // Element verändern
```

## Array durchlaufen

```csharp
int[] werte = { 10, 30, 20, 50, 40 };

// Minimum finden
int min = werte[0];
for (int i = 1; i < werte.Length; i++)
    if (werte[i] < min)
        min = werte[i];

Console.WriteLine($"Minimum: {min}"); // 10
```

## Häufiger Fehler: Index außerhalb des Bereichs

```csharp
int[] arr = new int[3]; // Indizes 0, 1, 2
arr[3] = 5;             // Laufzeitfehler! Index 3 existiert nicht.
```

Übung: Lies 5 Noten ein, speichere sie in einem Array und berechne Minimum, Maximum und Durchschnitt.
{: .notice--info}
