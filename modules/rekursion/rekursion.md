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

Zuerst der Algorithmus als Pseudocode:

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

## Fibonacci-Folge

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

## Rekursion vs. Schleife

Jede rekursive Lösung lässt sich auch iterativ (mit Schleifen) lösen. Rekursion ist oft eleganter, aber bei vielen Aufrufen speicherintensiver.

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

## Vorsicht: Endlose Rekursion

```csharp
static int Endlos(int n)
{
    return Endlos(n - 1); // Kein Basisfall → StackOverflowException!
}
```

Übung: Schreibe eine rekursive Methode `Summe(int n)`, die `1 + 2 + ... + n` berechnet.
{: .notice--info}

## Weitere Quellen

- [Rekursive Algorithmen visualisiert – VisuAlgo](https://visualgo.net/en/recursion)
