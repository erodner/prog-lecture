---
title: "🧩 Aufgaben und Beispiele: Operatoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Operatoren sind die Werkzeuge, mit denen wir Daten im Code verarbeiten. Aber echtes Verständnis entsteht erst, wenn man sie kreativ kombiniert. Die folgenden Aufgaben schulen dein *Computational Thinking* — insbesondere die Fähigkeit, mit einfachen Bausteinen clevere Lösungen zu konstruieren.

## Aufgabe 1 — Quersumme berechnen

Schreibe ein Programm, das die **Quersumme** einer beliebig langen positiven Ganzzahl berechnet — also die Summe aller Ziffern. Nutze nur die Operatoren `/` und `%` in einer Schleife.

**Beispiele:** Quersumme von 427 = 13, Quersumme von 9999 = 36, Quersumme von 1000000 = 1.

**Tipp:** Wie extrahierst du die letzte Ziffer? Wie entfernst du sie? Wann ist die Zahl „aufgebraucht"?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Idee:** Solange die Zahl größer als 0 ist, extrahieren wir die letzte Ziffer mit `% 10`, addieren sie zur Summe und entfernen sie mit `/ 10`.

**C#-Code:**

```csharp
int zahl = 427;
int summe = 0;

while (zahl > 0)
{
    int letzteZiffer = zahl % 10;
    summe += letzteZiffer;
    zahl = zahl / 10;
}

Console.WriteLine($"Quersumme: {summe}");
```

**Schritt-für-Schritt-Trace für 427:**

| Durchlauf | `zahl` | `zahl % 10` | `summe` | `zahl / 10` |
|-----------|--------|-------------|---------|-------------|
| 1 | 427 | 7 | 7 | 42 |
| 2 | 42 | 2 | 9 | 4 |
| 3 | 4 | 4 | 13 | 0 |
| Ende | 0 | — | **13** | — |

Die Schleife bricht ab, sobald `zahl` den Wert 0 erreicht. Die Quersumme steht in `summe`.

**Warum funktioniert das für beliebig lange Zahlen?** Die Schleife hat keine fest codierte Anzahl an Durchläufen — sie läuft so lange, wie Ziffern vorhanden sind. Egal ob die Zahl 3 Stellen oder 10 Stellen hat, der Algorithmus funktioniert identisch. Das ist ein gutes Beispiel dafür, wie man mit `/` und `%` systematisch alle Ziffern einer Zahl durchlaufen kann — ein Muster, das bei vielen Algorithmen (z.B. Zahlensysteme, Palindrom-Prüfung, Stellenwertanalyse) immer wieder auftaucht.

</details>

## Aufgabe 2 — Zahlen tauschen ohne Hilfsvariable

Gegeben sind zwei `int`-Variablen `a` und `b`. Tausche ihre Werte — **ohne eine dritte Variable** zu verwenden. Nutze nur arithmetische Operatoren.

```csharp
int a = 7;
int b = 3;
// Nach dem Tausch: a == 3, b == 7
```

Gibt es mehr als einen Weg? Welche Nachteile haben die Lösungen?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Methode 1 — Addition und Subtraktion:**

```csharp
int a = 7;
int b = 3;

a = a + b;   // a = 10, b = 3
b = a - b;   // a = 10, b = 7
a = a - b;   // a = 3,  b = 7
```

**Trace:**

| Schritt | Ausdruck | `a` | `b` |
|---------|----------|-----|-----|
| Start | — | 7 | 3 |
| 1 | `a = a + b` | 10 | 3 |
| 2 | `b = a - b` | 10 | 7 |
| 3 | `a = a - b` | 3 | 7 |

Der Trick: In Schritt 1 speichern wir die Summe in `a`. Daraus können wir in Schritt 2 den ursprünglichen Wert von `a` rekonstruieren (`Summe - b = altes a`) und in Schritt 3 den ursprünglichen Wert von `b` (`Summe - neues b = altes b`).

**Methode 2 — XOR (Exklusiv-Oder):**

```csharp
int a = 7;
int b = 3;

a = a ^ b;   // a = 4, b = 3
b = a ^ b;   // a = 4, b = 7
a = a ^ b;   // a = 3, b = 7
```

XOR hat die Eigenschaft, dass `x ^ x == 0` und `x ^ 0 == x`. Damit lässt sich der Tausch rein über Bitoperationen realisieren.

**Nachteile beider Methoden:**

- **Additions-Methode:** Kann bei sehr großen Werten zu einem **Ganzzahlüberlauf** führen, wenn `a + b` den Wertebereich von `int` sprengt.
- **XOR-Methode:** Funktioniert technisch einwandfrei, ist aber für die meisten Leserinnen und Leser **schwer verständlich**. Außerdem schlägt sie fehl, wenn `a` und `b` dieselbe Variable referenzieren (z.B. bei Arrays: `swap(arr, i, i)` würde den Wert auf 0 setzen).
- **Beide Methoden:** Kein moderner Compiler optimiert sie besser als eine Lösung mit Hilfsvariable.

**In der Praxis:** Immer eine Hilfsvariable verwenden! Das ist klar, sicher und wird vom Compiler genauso effizient umgesetzt:

```csharp
int temp = a;
a = b;
b = temp;
```

Der Wert dieser Aufgabe liegt nicht in der praktischen Anwendung, sondern darin, die Operatoren `+`, `-` und `^` wirklich zu durchdringen.

</details>
