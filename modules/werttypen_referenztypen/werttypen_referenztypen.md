---
title: "Werttypen und Referenztypen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bisher haben wir mit Variablen gearbeitet, ohne uns groß Gedanken darüber zu machen, was bei einer Zuweisung wie `b = a` eigentlich passiert. Bei `int` oder `double` ist das Verhalten intuitiv: `b` bekommt eine eigene Kopie des Werts. Bei Arrays verhält es sich anders — und das überrascht viele Anfänger. Der Unterschied zwischen Werttypen und Referenztypen ist eines der wichtigsten Konzepte in C# und erklärt viele auf den ersten Blick rätselhafte Verhaltensweisen.

## Werttypen — die Variable *ist* der Wert

Bei **Werttypen** speichert die Variable den Wert direkt. Eine Zuweisung erzeugt eine vollständig **unabhängige Kopie** — danach haben die beiden Variablen nichts mehr miteinander zu tun:

```csharp
int a = 10;
int b = a;   // b bekommt eine eigenständige Kopie
b = 99;

Console.WriteLine(a); // 10 — unverändert!
Console.WriteLine(b); // 99
```

Man kann sich das so vorstellen: Jede Variable ist ein eigenes Kästchen, das seinen Wert enthält. Wenn man `b = a` schreibt, wird der Inhalt von Kästchen `a` in Kästchen `b` kopiert. Danach sind es zwei getrennte Kästchen.

```
a: ┌────┐     b: ┌────┐
   │ 10 │        │ 10 │  ← Kopie des Werts
   └────┘        └────┘
```

Zu den Werttypen gehören alle einfachen numerischen Typen (`int`, `double`, `float`, `long`, `byte`, `short`, `decimal`), außerdem `bool`, `char` und `struct`.

## Referenztypen — die Variable *zeigt auf* den Wert

Bei **Referenztypen** speichert die Variable nicht den Wert selbst, sondern eine **Referenz** — eine Art Adresse, die auf den eigentlichen Inhalt im Speicher zeigt. Eine Zuweisung kopiert nur diese Referenz, **nicht die Daten dahinter**. Beide Variablen zeigen danach auf dasselbe Objekt:

```csharp
int[] x = { 1, 2, 3 };
int[] y = x;     // y zeigt auf dasselbe Array wie x

y[0] = 99;

Console.WriteLine(x[0]); // 99 — x wurde mitverändert!
Console.WriteLine(y[0]); // 99
```

Das ist der entscheidende Unterschied: Es gibt nur **ein** Array im Speicher, aber **zwei** Variablen, die darauf zeigen. Eine Änderung über `y` ist sofort auch über `x` sichtbar, weil beide auf denselben Speicherbereich verweisen.

```
x ──────────→  ┌────┬────┬────┐
               │ 99 │  2 │  3 │
y ──────────→  └────┴────┴────┘
```

Zu den Referenztypen gehören Arrays, `string` und Klassen (die wir später kennenlernen).

## Warum ist das so?

Der Grund ist Effizienz. Ein `int` belegt 4 Byte — das zu kopieren ist billig. Ein Array mit einer Million Elementen belegt mehrere Megabyte. Würde jede Zuweisung eine vollständige Kopie erzeugen, wäre das extrem langsam und speicherintensiv. Stattdessen wird nur die Referenz kopiert (die selbst nur wenige Byte groß ist), und beide Variablen teilen sich die Daten.

## Ein echtes Duplikat erstellen

Wenn man tatsächlich eine unabhängige Kopie eines Arrays braucht, muss man das explizit anfordern. Die Methode `Clone()` erzeugt ein neues Array mit denselben Werten:

```csharp
int[] x = { 1, 2, 3 };
int[] y = (int[])x.Clone();  // echte Kopie

y[0] = 99;

Console.WriteLine(x[0]); // 1 — unverändert
Console.WriteLine(y[0]); // 99
```

Nach dem `Clone()` existieren zwei getrennte Arrays im Speicher. Änderungen an einem wirken sich nicht mehr auf das andere aus.

## Der Sonderfall `string`

Strings sind technisch Referenztypen, verhalten sich aber im Alltag wie Werttypen. Der Grund: Strings sind **unveränderlich** (*immutable*). Jede Operation, die einen String „verändert", erzeugt in Wirklichkeit einen neuen String. Deshalb kann man mit Strings arbeiten, ohne sich um geteilte Referenzen sorgen zu müssen:

```csharp
string a = "Hallo";
string b = a;
b = "Welt";

Console.WriteLine(a); // "Hallo" — unverändert
Console.WriteLine(b); // "Welt"
```

Die Zuweisung `b = "Welt"` lässt `b` auf einen neuen String zeigen — `a` zeigt weiterhin auf `"Hallo"`. Das ist ein Unterschied zu Arrays, wo `y[0] = 99` den geteilten Inhalt verändert.

## Das typische Missverständnis

Der häufigste Fehler ist die Annahme, dass `int[] y = x` eine Kopie des Arrays erzeugt. Dieses Missverständnis führt zu Bugs, die schwer zu finden sind, weil das Programm kompiliert und läuft — nur die Ergebnisse stimmen nicht:

```csharp
int[] original = { 10, 20, 30 };
int[] backup = original;      // KEIN Backup! Nur eine zweite Referenz

original[0] = 0;
Console.WriteLine(backup[0]); // 0 — das "Backup" wurde mitverändert
```

Wenn man ein echtes Backup braucht, muss man `Clone()` verwenden.
{: .notice--warning}


## Weitere Quellen

- [Werttypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/value-types)
- [Referenztypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/reference-types)
- [Werttypen und Referenztypen (Übersicht) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types)
- [Heap und Stack in C# – C# Corner](https://www.c-sharpcorner.com/article/C-Sharp-heaping-vs-stacking-in-net-part-i/)
- [Value vs. Reference Types (Video) – dotnet](https://www.youtube.com/watch?v=bWGJIkOQQXQ)
