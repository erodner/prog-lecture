---
title: "Rekursion"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Stell dir vor, du erklärst jemandem, wie man eine Treppe hinuntergeht: „Geh eine Stufe runter – und dann geh die restliche Treppe runter." Das ist Rekursion: ein Problem lösen, indem man es auf eine kleinere Version desselben Problems zurückführt. Elegant, aber man braucht immer einen Punkt, an dem es aufhört.

Rekursion ist **Algorithmenentwicklung** (*Algorithm Design*) in besonders klarer Form – eines der vier Grundprinzipien des Computational Thinking. Der Trick liegt darin, eine präzise Handlungsvorschrift zu finden, die das Problem schrittweise verkleinert und einen klaren Endpunkt definiert. Das lässt sich gut im Pseudocode durchdenken, bevor man auch nur eine Zeile Code schreibt.

## Was ist Rekursion?

Eine Methode ist **rekursiv**, wenn sie sich selbst aufruft. Jede rekursive Methode braucht:
1. einen **Basisfall** – der den Aufruf stoppt
2. einen **rekursiven Fall** – der das Problem verkleinert

## Klassisches Beispiel: Fakultät

Die Fakultät von `n` (geschrieben `n!`) ist das Produkt aller Zahlen von 1 bis n: `5! = 5 × 4 × 3 × 2 × 1 = 120`. Rekursiv lässt sich das elegant formulieren: `n! = n × (n-1)!` — die Fakultät von 5 ist 5 mal die Fakultät von 4. Zuerst der Algorithmus als Pseudocode:

```
fakultät(n):
    wenn n <= 1: gib 1 zurück          // Basisfall
    sonst:       gib n × fakultät(n-1) zurück  // rekursiver Fall
```

Die Expansion zeigt, wie sich das Problem verkleinert:

```
5! = 5 × 4! = 5 × 4 × 3! = 5 × 4 × 3 × 2! = 5 × 4 × 3 × 2 × 1
```

```csharp
static long Fakultät(int n)
{
    if (n <= 1) return 1;          // Basisfall
    return n * Fakultät(n - 1);   // rekursiver Fall
}

Console.WriteLine(Fakultät(5));   // 120
Console.WriteLine(Fakultät(10));  // 3628800
```

## Was passiert im Speicher?

Bei jedem Methodenaufruf legt C# einen neuen Eintrag auf dem **Call Stack** an — mit den lokalen Variablen und der Rücksprungadresse. Bei `Fakultät(4)` sieht der Stack so aus:

```
Fakultät(1) → gibt 1 zurück        ← oberster Eintrag
Fakultät(2) → wartet auf Fakultät(1)
Fakultät(3) → wartet auf Fakultät(2)
Fakultät(4) → wartet auf Fakultät(3)
Main()                              ← unterster Eintrag
```

Sobald `Fakultät(1)` den Basisfall erreicht und `1` zurückgibt, werden die wartenden Aufrufe von oben nach unten abgearbeitet: `1 → 2×1=2 → 3×2=6 → 4×6=24`. Dieses Aufstapeln und Abbauen ist der Kern jeder Rekursion.

## Fibonacci-Folge

Die Fibonacci-Folge ist ein weiteres klassisches Beispiel: Jede Zahl ist die Summe der beiden vorherigen (`0, 1, 1, 2, 3, 5, 8, 13, ...`). Die rekursive Definition ist besonders elegant, weil sie die mathematische Formel fast wörtlich übersetzt:

```csharp
static int Fibonacci(int n)
{
    if (n <= 1) return n;                        // Basisfall
    return Fibonacci(n - 1) + Fibonacci(n - 2); // rekursiver Fall
}

for (int i = 0; i < 10; i++)
    Console.Write(Fibonacci(i) + " ");
// 0 1 1 2 3 5 8 13 21 34
```

Diese Implementierung hat allerdings ein Problem: Sie ist extrem langsam für große `n`. `Fibonacci(5)` ruft `Fibonacci(4)` und `Fibonacci(3)` auf — aber `Fibonacci(4)` ruft seinerseits nochmal `Fibonacci(3)` auf. Dieselben Werte werden wieder und wieder berechnet. Für `Fibonacci(40)` entstehen über eine Milliarde Aufrufe. Das zeigt: Eleganz und Effizienz sind nicht dasselbe.
{: .notice--warning}

## Rekursion vs. Schleife

Das Fibonacci-Beispiel zeigt: Eine rekursive Lösung kann mathematisch elegant sein und trotzdem in der Praxis untauglich, weil sie dieselben Teilprobleme immer wieder neu berechnet. Das wirft eine grundsätzliche Frage auf – muss es überhaupt Rekursion sein?

Jede rekursive Lösung lässt sich auch iterativ (mit Schleifen) lösen — und umgekehrt. Die iterative Variante ist oft schneller und speicherschonender, weil sie keinen wachsenden Call Stack aufbaut:

```csharp
// Iterative Fakultät:
static long FakultätIterativ(int n)
{
    long ergebnis = 1;
    for (int i = 2; i <= n; i++)
        ergebnis *= i;
    return ergebnis;
}
```

Wann lohnt sich Rekursion trotzdem? Wenn das Problem eine natürlich rekursive Struktur hat — z.B. Baumstrukturen, Dateisysteme oder das Durchsuchen verschachtelter Daten. In solchen Fällen ist die rekursive Lösung oft deutlich kürzer und verständlicher als eine iterative mit explizitem Stack. Ein Verzeichnisbaum auf der Festplatte etwa enthält Unterverzeichnisse, die wiederum Unterverzeichnisse enthalten — eine rekursive Methode `DurchsucheVerzeichnis` ruft sich für jedes Unterverzeichnis selbst auf und bildet damit die Struktur der Daten direkt im Code ab.

## Vorsicht: Endlose Rekursion

Fehlt der Basisfall oder verkleinert der rekursive Aufruf das Problem nicht, läuft die Rekursion endlos — bis der Call Stack voll ist und das Programm mit einer `StackOverflowException` abstürzt:

```csharp
static int Endlos(int n)
{
    return Endlos(n - 1); // Kein Basisfall → StackOverflowException!
}
```

Der Call Stack hat eine begrenzte Größe (typischerweise 1–4 MB). Deshalb gilt: Bei jedem rekursiven Aufruf sicherstellen, dass (1) ein Basisfall existiert und (2) jeder Aufruf dem Basisfall näherkommt.
{: .notice--warning}

Übung: Schreibe eine rekursive Methode `Summe(int n)`, die `1 + 2 + ... + n` berechnet. Überlege dir zuerst den Basisfall und den rekursiven Fall.
{: .notice--info}

## Weitere Quellen

- [Rekursion – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/methods#recursive-methods)
- [Rekursive Algorithmen visualisiert – VisuAlgo](https://visualgo.net/en/recursion)
- [Recursion in Programming (Video) – Computerphile](https://www.youtube.com/watch?v=Mv9NEXX1VHc)
