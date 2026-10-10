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

Schreibe ein Programm, das die **Quersumme** einer **vierstelligen** positiven Ganzzahl berechnet — also die Summe aller Ziffern. Verwende dabei nur die Operatoren `/` und `%`, keine Umwandlung in einen String.

**Beispiele:** Quersumme von 4271 = 14, Quersumme von 9999 = 36, Quersumme von 1000 = 1.

**Tipp:** Was liefert `4271 % 10`? Was liefert `4271 / 10`? Und was `4271 / 100 % 10`?

Findest du zwei verschiedene Lösungswege? Woran scheitern sie, wenn die Zahl plötzlich fünf- oder zehnstellig sein darf?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Weg 1 — jede Stelle gezielt herausrechnen:** Durch ganzzahlige Division schiebt man die gewünschte Ziffer an die letzte Stelle, mit `% 10` schneidet man alles davor ab:

```csharp
int zahl = 4271;

int tausender = zahl / 1000;        // 4
int hunderter = zahl / 100 % 10;    // 42 % 10 = 2
int zehner    = zahl / 10 % 10;     // 427 % 10 = 7
int einer     = zahl % 10;          // 1

Console.WriteLine($"Quersumme: {tausender + hunderter + zehner + einer}");  // 14
```

`/` und `%` haben dieselbe Rangfolge und werden von links nach rechts ausgewertet — `zahl / 100 % 10` bedeutet also `(zahl / 100) % 10`.

**Weg 2 — immer die letzte Ziffer abknabbern:** Statt jede Stelle einzeln zu adressieren, nimmt man stets die letzte Ziffer (`% 10`) und entfernt sie danach (`/ 10`):

```csharp
int zahl = 4271;
int summe = 0;

summe += zahl % 10;  zahl /= 10;   // summe = 1,  zahl = 427
summe += zahl % 10;  zahl /= 10;   // summe = 8,  zahl = 42
summe += zahl % 10;  zahl /= 10;   // summe = 10, zahl = 4
summe += zahl % 10;  zahl /= 10;   // summe = 14, zahl = 0

Console.WriteLine($"Quersumme: {summe}");
```

**Woran scheitern beide Wege bei längeren Zahlen?** Beide sind fest auf vier Stellen zugeschnitten. Bei Weg 1 bräuchte man für jede weitere Stelle eine neue Zeile mit einem neuen Divisor — und man müsste die Stellenzahl vorher kennen. Weg 2 ist interessanter: Er besteht aus **viermal exakt derselben Zeile**. Und am Ende ist `zahl` genau 0 — ein natürliches Signal, dass alle Ziffern verarbeitet sind. Wenn man „wiederhole diese Zeile, solange `zahl` größer als 0 ist" ausdrücken könnte, würde der Algorithmus für beliebig lange Zahlen funktionieren. Genau das ermöglichen Schleifen, die wir in Vorlesung 6 kennenlernen — dort greifen wir die Quersumme in den [Schleifen-Aufgaben](/modules/aufgaben_schleifen/aufgaben_schleifen.md) wieder auf.

Das Muster aus `% 10` und `/ 10` taucht bei vielen Algorithmen (Zahlensysteme, Palindrom-Prüfung, Stellenwertanalyse) immer wieder auf.

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
