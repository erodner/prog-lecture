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

Die `for`-Schleife ist die klassische Wahl, wenn man den **Index** beim Durchlaufen braucht — etwa um die Position im Array anzuzeigen, Elemente zu verändern oder das Array rückwärts zu durchlaufen:

```csharp
int[] temperaturen = { 18, 22, 17, 25, 21, 19, 24 };

// Alle Temperaturen ausgeben mit Tag-Nummer
for (int i = 0; i < temperaturen.Length; i++)
    Console.WriteLine($"Tag {i + 1}: {temperaturen[i]}°C");

// Array rückwärts ausgeben
for (int i = temperaturen.Length - 1; i >= 0; i--)
    Console.Write(temperaturen[i] + " ");
```

Das Muster `for (int i = 0; i < arr.Length; i++)` wird man in C#-Programmen hundertfach sehen — es durchläuft jedes Element von vorne nach hinten. Bei rückwärts-Durchlauf startet man bei `Length - 1` und zählt herunter bis `0`.

## `foreach` — über alle Elemente

Wenn alle Elemente der Reihe nach verarbeitet werden sollen und der Index nicht benötigt wird, bietet `foreach` eine kompaktere und lesbarere Alternative. Die Schleife übernimmt die Indexverwaltung automatisch und liefert bei jedem Durchlauf das nächste Element:

```csharp
foreach (Typ element in Array)
{
    // element enthält den aktuellen Wert
}
```

Mit `foreach` kann man Elemente nur **lesen**, nicht verändern — für Schreibzugriff muss `for` mit Index genutzt werden.
{: .notice--warning}

Ein typischer Anwendungsfall ist das Aufsummieren aller Werte. Hier braucht man keinen Index, sondern nur die Werte selbst:

```csharp
double summe = 0;
foreach (int t in temperaturen)
    summe += t;

Console.WriteLine($"Durchschnitt: {summe / temperaturen.Length:F1}°C");
```

**Faustregel:** `foreach` verwenden, wenn man nur lesen will und keinen Index braucht. `for` verwenden, wenn man den Index braucht, Elemente verändern will oder nicht alle Elemente durchlaufen soll.
{: .notice--primary}

## Array befüllen mit Schleife

Arrays müssen nicht von Hand befüllt werden — oft berechnet man die Werte aus dem Index. Im folgenden Beispiel werden die ersten zehn Quadratzahlen erzeugt. Da das Array verändert wird, ist hier `for` mit Index nötig:

```csharp
int[] quadrate = new int[10];
for (int i = 0; i < quadrate.Length; i++)
    quadrate[i] = (i + 1) * (i + 1);

// Ausgabe: 1 4 9 16 25 36 49 64 81 100
foreach (int q in quadrate)
    Console.Write(q + " ");
```

Man sieht hier auch ein häufiges Zusammenspiel: `for` zum Befüllen (Schreibzugriff), `foreach` zum Ausgeben (nur Lesen).

## Häufigster Wert — ein Array als Zähler

Eine elegante Technik ist die Verwendung eines Arrays als **Zähler**: Der Index repräsentiert den Wert, und der Inhalt zählt, wie oft dieser Wert vorkommt. Bevor man Code schreibt, lohnt es sich, den Algorithmus als Pseudocode zu formulieren:

```
häufigsterWert(würfe):
    lege ein Zählarray für alle möglichen Werte an
    für jeden Wurf: erhöhe den Zähler für diesen Wert
    durchsuche das Zählarray nach dem Maximum
    gib den Index des Maximums zurück
```

In C# sieht das so aus. Das Zählarray `häufigkeit` hat so viele Einträge wie es mögliche Werte gibt. Mit `häufigkeit[w]++` wird der Zähler an der Stelle `w` um 1 erhöht — das ist die zentrale Idee:

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

Dieses Muster (oft *Frequency Counting* oder *Histogramm* genannt) begegnet einem in der Praxis ständig — von der Textanalyse bis zur Statistik.

Übung: Generiere ein Array mit 20 Zufallszahlen zwischen 1 und 6 (`Random.Shared.Next(1, 7)`) und zähle, wie oft jede Zahl vorkommt.
{: .notice--info}

## Weitere Quellen

- [Arrays und foreach – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/arrays/using-foreach-with-arrays)
- [Array-Algorithmen visualisiert – VisuAlgo](https://visualgo.net/en/sorting)
