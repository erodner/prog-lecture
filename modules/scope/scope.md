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

## Call by Reference mit `ref`

Call by Value ist sicher und vorhersagbar – aber manchmal reicht es nicht aus. Was, wenn eine Methode nicht nur ein Ergebnis berechnen, sondern eine bestehende Variable direkt verändern soll? Zum Beispiel eine Tausch-Operation, die zwei Variablen vertauscht – mit Kopien wäre das unmöglich. Dafür gibt es `ref` — die Methode bekommt dann eine Referenz auf die originale Variable, nicht auf eine Kopie:

```csharp
static void Verdoppeln(ref int x)
{
    x *= 2;
}

int zahl = 10;
Verdoppeln(ref zahl);
Console.WriteLine(zahl); // 20 – geändert!
```

Das `ref`-Schlüsselwort muss sowohl in der Methodensignatur als auch beim Aufruf stehen – das ist bewusst so, damit es keine versteckten Seiteneffekte gibt. Wer `ref` sieht, weiß sofort: diese Variable kann nach dem Aufruf einen anderen Wert haben.

`ref` sollte sparsam eingesetzt werden. Meist ist es klarer, einen Wert per `return` zurückzugeben, statt eine Variable von außen zu verändern.
{: .notice--primary}

## Ausgabe-Parameter mit `out`

Eine Methode kann nur einen Wert per `return` zurückgeben. Was tun, wenn man mehrere Ergebnisse braucht? Ein typisches Beispiel: Eine Dauer in Minuten soll in Stunden und Restminuten zerlegt werden – mit den Operatoren `/` und `%` aus der Vorlesung zu [arithmetischen Operatoren](/modules/operatoren_arithmetik/operatoren_arithmetik.md) ist das schnell gerechnet, aber das Ergebnis besteht aus *zwei* Zahlen.

Eine Möglichkeit bietet `out`: Die Methode schreibt ihre Ergebnisse direkt in die angegebenen Variablen. Anders als `ref` muss die Variable vorher nicht initialisiert sein — die Methode ist verpflichtet, ihr einen Wert zuzuweisen:

```csharp
static void InStundenUndMinuten(int gesamtMinuten, out int stunden, out int minuten)
{
    stunden = gesamtMinuten / 60;
    minuten = gesamtMinuten % 60;
}

InStundenUndMinuten(135, out int h, out int m);
Console.WriteLine($"{h} h {m} min"); // 2 h 15 min
```

Ein bekanntes Beispiel aus der BCL ist `int.TryParse`, das wir bei der [Typkonvertierung](/modules/typkonvertierung/typkonvertierung.md) schon benutzt haben: Es gibt per `return` einen `bool` zurück (Erfolg ja/nein) und schreibt das geparste Ergebnis per `out` in eine Variable.

## Mehrere Rückgabewerte mit Tupeln

Mit `out` lassen sich zwar mehrere Werte zurückgeben, aber die Syntax ist etwas umständlich und der Aufrufer muss die `out`-Variablen explizit deklarieren. Seit C# 7 gibt es eine elegantere Alternative: Eine modernere und oft lesbarere Alternative zu `out` ist die Rückgabe eines **Tupels**. Ein Tupel fasst mehrere Werte zu einem einzigen Rückgabetyp zusammen, ohne dass man dafür eine eigene Klasse definieren muss:

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

Verglichen mit der `out`-Variante steht das Ergebnis jetzt dort, wo man es erwartet: links vom `=`.

Tupel eignen sich gut, wenn eine Methode zwei bis drei zusammengehörige Werte zurückgibt. Für komplexere Datenstrukturen mit eigenem Verhalten ist eine Klasse die bessere Wahl.
{: .notice--primary}

Übung: Schreibe eine Methode `Sortiere(int a, int b, int c)`, die die drei Zahlen als Tupel `(int kleinste, int mittlere, int größte)` zurückgibt. Schreibe anschließend eine Methode `SortiereVariablen(ref int a, ref int b, ref int c)`, die die drei Variablen des Aufrufers direkt in aufsteigende Reihenfolge bringt – mit einer Hilfsmethode `Tausche(ref int x, ref int y)`. Welche der beiden Varianten würdest du lieber benutzen?
{: .notice--info}

## Weitere Quellen

- [Gültigkeitsbereich von Variablen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/scope)
- [Parameterübergabe – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/passing-parameters)
- [ref und out – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/ref)
- [Tupel in C# – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/value-tuples)
