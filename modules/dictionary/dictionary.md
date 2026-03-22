---
title: "Dictionary<K,V> – Schlüssel-Wert-Paare"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal braucht man nicht nur eine Liste von Werten, sondern eine Zuordnung: welche Stadt hat wie viele Einwohner, welches Wort kommt wie oft vor? Ein `Dictionary` ist genau das – eine Sammlung von Schlüssel-Wert-Paaren, bei der der Zugriff über den Schlüssel blitzschnell erfolgt.

## Was ist ein Dictionary?

Ein `Dictionary<TKey, TValue>` speichert Paare aus **Schlüssel** und **Wert**. Der Zugriff auf einen Wert erfolgt über seinen Schlüssel – ähnlich wie ein Wörterbuch.

```
Schlüssel (Key)  →  Wert (Value)
"Berlin"         →  3_600_000
"Hamburg"        →  1_800_000
"München"        →  1_500_000
```

## Grundoperationen

```csharp
Dictionary<string, int> einwohner = new Dictionary<string, int>();

// Hinzufügen
einwohner["Berlin"]   = 3_600_000;
einwohner["Hamburg"]  = 1_800_000;
einwohner["München"]  = 1_500_000;

// Lesen
Console.WriteLine(einwohner["Berlin"]); // 3600000

// Prüfen ob Schlüssel existiert (wichtig – sonst Exception!)
if (einwohner.ContainsKey("Köln"))
    Console.WriteLine(einwohner["Köln"]);

// TryGetValue – sicherer Zugriff
if (einwohner.TryGetValue("Hamburg", out int ew))
    Console.WriteLine($"Hamburg: {ew}");

// Entfernen
einwohner.Remove("München");

Console.WriteLine(einwohner.Count); // 2
```

## Iteration

```csharp
foreach (KeyValuePair<string, int> paar in einwohner)
    Console.WriteLine($"{paar.Key}: {paar.Value:N0}");

// Kurzform:
foreach (var (stadt, anzahl) in einwohner)
    Console.WriteLine($"{stadt}: {anzahl:N0}");
```

## Praxisbeispiel: Worthäufigkeit

```csharp
string text = "der die das der ein eine der";
string[] wörter = text.Split(' ');

Dictionary<string, int> häufigkeit = new();

foreach (string wort in wörter)
{
    if (häufigkeit.ContainsKey(wort))
        häufigkeit[wort]++;
    else
        häufigkeit[wort] = 1;
}

foreach (var (wort, anzahl) in häufigkeit)
    Console.WriteLine($"{wort,10}: {anzahl}");
```

Übung: Lies einen Satz ein und zähle, wie oft jeder Buchstabe (Groß-/Kleinschreibung ignorieren) vorkommt.
{: .notice--info}

## Weitere Quellen

- [Dictionary\<TKey,TValue\> – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.collections.generic.dictionary-2)
- [Hash-Tabellen visualisiert – VisuAlgo](https://visualgo.net/en/hashtable)
