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

Der Zugriff über eckige Klammern mit dem Schlüssel sieht ähnlich aus wie bei Arrays — nur dass der „Index" kein `int` sein muss, sondern z.B. ein `string`. Wichtig: Greift man auf einen Schlüssel zu, der nicht existiert, wirft das Dictionary eine `KeyNotFoundException`:

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

`TryGetValue` ist die sicherere Alternative zu `ContainsKey` + Zugriff: Es prüft und liefert den Wert in einem Schritt — das Muster kennen wir bereits von `int.TryParse`.

## Iteration

Neben dem gezielten Zugriff über einzelne Schlüssel möchte man häufig alle Einträge eines Dictionary durchlaufen — etwa um eine Übersicht auszugeben. Beim Durchlaufen eines Dictionary erhält man `KeyValuePair`-Objekte mit `.Key` und `.Value`. Die Kurzform mit Dekonstruktion (`var (stadt, anzahl)`) ist lesbarer:

```csharp
foreach (KeyValuePair<string, int> paar in einwohner)
    Console.WriteLine($"{paar.Key}: {paar.Value:N0}");

// Kurzform:
foreach (var (stadt, anzahl) in einwohner)
    Console.WriteLine($"{stadt}: {anzahl:N0}");
```

## Praxisbeispiel: Worthäufigkeit

Nachdem wir die Grundoperationen und die Iteration kennengelernt haben, wenden wir beides in einem klassischen Anwendungsfall an. Das Zählen von Häufigkeiten ist eine der klassischen Anwendungen für Dictionaries — das kennen wir als Muster bereits von Arrays (Frequency Counting), nur dass der Schlüssel jetzt ein beliebiger Typ sein kann statt nur ein Index:

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

**Warum ist Dictionary so schnell?** Intern verwendet ein Dictionary eine Hash-Tabelle. Das bedeutet: Der Zugriff auf einen Wert über seinen Schlüssel hat im Durchschnitt eine Zeitkomplexität von **O(1)** — also konstant, unabhängig von der Größe des Dictionary. Zum Vergleich: In einer `List<T>` müsste man im schlimmsten Fall alle Elemente durchsuchen (**O(n)**). Deshalb ist ein Dictionary die richtige Wahl, wenn häufig nach Schlüsseln gesucht wird.
{: .notice--primary}

## Weitere Quellen

- [Dictionary\<TKey,TValue\> – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.collections.generic.dictionary-2)
- [Hash-Tabellen visualisiert – VisuAlgo](https://visualgo.net/en/hashtable)
