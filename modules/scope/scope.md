---
title: "Gültigkeitsbereiche und Parameterübergabe"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Wenn Methoden Daten austauschen, stellen sich zwei grundlegende Fragen: Welche Variablen sind wo sichtbar? Und was passiert genau, wenn man einer Methode einen Wert übergibt – arbeitet sie mit dem Original oder mit einer Kopie? Diese Fragen sind entscheidend, um Fehler zu vermeiden, die auf den ersten Blick rätselhaft wirken.

## Gültigkeitsbereiche (Scope)

Jede Variable existiert nur innerhalb des Blocks `{ }`, in dem sie deklariert wurde. Sobald der Block endet, ist die Variable verschwunden – sie ist nicht mehr zugänglich und ihr Speicher wird freigegeben.

```csharp
void Beispiel()
{
    int x = 10;

    if (x > 5)
    {
        int y = 20;               // y lebt nur in diesem if-Block
        Console.WriteLine(x + y); // OK: x ist aus dem äußeren Block sichtbar
    }

    Console.WriteLine(y); // Compilerfehler! y existiert hier nicht mehr
}
```

Das klingt zunächst wie eine Einschränkung, ist aber ein Schutzmechanismus: Man kann nicht versehentlich eine Variable aus einem anderen Kontext verändern. Jede Methode hat ihren eigenen, isolierten Bereich – Variablen aus `Main` sind in `Kreisfläche` nicht sichtbar, und umgekehrt. Genau das ermöglicht es, Methoden unabhängig voneinander zu entwickeln und zu testen.

## Parameterübergabe: Call by Value

Scope erklärt, welche Variablen sichtbar sind. Aber was passiert mit den Werten, die man einer Methode übergibt? Werden sie kopiert, oder arbeitet die Methode mit dem Original?

Bei den Typen, die wir bisher kennen (`int`, `double`, `bool`, `char` usw.), bekommt die Methode standardmäßig eine **Kopie** — nicht das Original. Der Parameter ist eine neue, eigene Variable, die beim Aufruf mit dem übergebenen Wert befüllt wird. Änderungen innerhalb der Methode bleiben dort, ohne auf die aufrufende Stelle durchzuschlagen:

```csharp
static void Verdoppeln(int x)
{
    x *= 2;
    Console.WriteLine($"In Methode: {x}"); // 20
}

int zahl = 10;
Verdoppeln(zahl);
Console.WriteLine($"Nach Aufruf: {zahl}"); // 10 – unverändert!
```

Der Vorteil: Man kann einer Methode Werte übergeben, ohne befürchten zu müssen, dass sie diese unbeabsichtigt verändert. Die Methode ist eine Black Box – sie bekommt ihre Eingabe und liefert eine Ausgabe, ohne Seiteneffekte.

## Mehrere Rückgabewerte mit Tupeln

Call by Value hat eine Konsequenz: Eine Methode kann nicht einfach Variablen des Aufrufers befüllen. Ihr einziger Weg, ein Ergebnis zu liefern, ist `return` – und damit lässt sich nur *ein* Wert zurückgeben. Was tun, wenn man mehrere Ergebnisse braucht? Ein typisches Beispiel: Eine Dauer in Minuten soll in Stunden und Restminuten zerlegt werden. Mit den Operatoren `/` und `%` aus der Vorlesung zu [arithmetischen Operatoren](/modules/operatoren_arithmetik/operatoren_arithmetik.md) ist das schnell gerechnet, aber das Ergebnis besteht aus *zwei* Zahlen.

Die Lösung ist ein **Tupel**: Es fasst mehrere Werte zu einem einzigen Rückgabetyp zusammen, ohne dass man dafür eine eigene Klasse definieren muss:

```csharp
static (int stunden, int minuten) InStundenUndMinuten(int gesamtMinuten)
{
    return (gesamtMinuten / 60, gesamtMinuten % 60);
}
```

Der Aufrufer kann die Rückgabe entweder als ganzes Tupel speichern oder direkt in einzelne Variablen **dekonstruieren**. Hier ist einer der Fälle, in denen sich `var` lohnt (siehe [Datentypen](/modules/datentypen/datentypen.md)): Statt `(int stunden, int minuten) dauer` überlassen wir den Tupeltyp dem Compiler:

```csharp
// Als Tupel:
var dauer = InStundenUndMinuten(135);
Console.WriteLine($"{dauer.stunden} h {dauer.minuten} min"); // 2 h 15 min

// Dekonstruiert in einzelne Variablen:
var (h, m) = InStundenUndMinuten(135);
Console.WriteLine($"{h} h {m} min"); // 2 h 15 min
```

Tupel eignen sich gut, wenn eine Methode zwei bis drei zusammengehörige Werte zurückgibt. Für komplexere Datenstrukturen mit eigenem Verhalten ist eine Klasse die bessere Wahl.
{: .notice--primary}

Übung: Schreibe eine Methode `Sortiere(int a, int b, int c)`, die die drei Zahlen als Tupel `(int kleinste, int mittlere, int größte)` zurückgibt. Rufe sie auf und gib das Ergebnis dekonstruiert aus.
{: .notice--info}

Übung: Eine Kommilitonin schreibt `static void Tausche(int x, int y) { int tmp = x; x = y; y = tmp; }` und wundert sich, dass nach `Tausche(a, b)` die Variablen `a` und `b` unverändert sind. Erkläre mit Call by Value, was passiert. Wie könntest du das Vertauschen stattdessen mit einem Tupel als Rückgabewert lösen?
{: .notice--info}

## Weitere Quellen

- [Gültigkeitsbereich von Variablen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/scope)
- [Parameterübergabe – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/passing-parameters)
- [Tupel in C# – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/value-tuples)
