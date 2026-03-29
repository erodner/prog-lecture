---
title: "🧩 Aufgaben und Beispiele: Typen und Variablen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmieren lernt man nicht nur durch Codezeilen tippen — sondern auch durch **Nachdenken**. Die folgenden Aufgaben trainieren *Computational Thinking*: die Fähigkeit, Probleme so zu strukturieren, dass ein Computer sie lösen kann. Dazu gehören Abstraktion, Zerlegung, Mustererkennung und Algorithmenentwurf. Nimm dir für jede Aufgabe Zeit, bevor du die Lösung aufklappst.

## Aufgabe 1 — Die Tücken der Gleitkommazahlen

Was gibt das folgende Programm aus? Überlege zuerst, bevor du es ausführst:

```csharp
double a = 0.1 + 0.2;
Console.WriteLine(a == 0.3);        // ???
Console.WriteLine(a);               // ???
Console.WriteLine(0.3 - 0.1 - 0.1 - 0.1);  // ???
```

Warum sind die Ergebnisse überraschend? Was bedeutet das für Vergleiche mit `==` bei `double`?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Ausgabe:**

```
False
0.30000000000000004
-2.7755575615628914E-17
```

**Erklärung:** Computer speichern Gleitkommazahlen im IEEE-754-Format als *Binärbrüche*. Genau wie man im Dezimalsystem den Bruch 1/3 nicht exakt darstellen kann (0,3333...), kann das Binärsystem die Zahl 0,1 nicht exakt darstellen. Intern wird 0,1 zu einer minimal abweichenden Zahl gerundet.

Deshalb ist `0.1 + 0.2` nicht exakt `0.3`, sondern `0.30000000000000004`. Der Vergleich `a == 0.3` liefert `False`, weil die winzige Abweichung ausreicht, um die Gleichheit zu verletzen.

Der dritte Ausdruck `0.3 - 0.1 - 0.1 - 0.1` sollte mathematisch `0.0` ergeben, liefert aber `-2.78E-17` — eine Zahl extrem nahe an Null, aber eben nicht Null. Die Rundungsfehler summieren sich bei jeder Operation auf.

**Konsequenz:** Man sollte `double`-Werte **niemals** direkt mit `==` vergleichen. Stattdessen prüft man, ob die Differenz kleiner als ein kleiner Schwellenwert (Epsilon) ist:

```csharp
double a = 0.1 + 0.2;
double b = 0.3;

// Sicherer Vergleich:
bool sindGleich = Math.Abs(a - b) < 1e-10;
Console.WriteLine(sindGleich);  // True
```

**Warum gibt es `decimal`?** Für Finanzberechnungen, bei denen exakte Dezimalwerte nötig sind, bietet C# den Typ `decimal`. Dieser speichert Zahlen als Dezimalbrüche (nicht Binärbrüche) und kann Werte wie `0.1` exakt darstellen — allerdings auf Kosten von Geschwindigkeit und Speicher.

```csharp
decimal x = 0.1m + 0.2m;
Console.WriteLine(x == 0.3m);  // True
```

</details>

## Aufgabe 2 — Ganzzahlüberlauf

Was passiert bei diesem Code? Überlege, welchen Wert `ergebnis` hat — und warum:

```csharp
int max = int.MaxValue;   // 2.147.483.647
int ergebnis = max + 1;
Console.WriteLine(ergebnis);
```

Und was passiert, wenn man denselben Code mit `checked` umklammert? Wann könnte ein Überlauf in der Praxis gefährlich werden?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Ausgabe ohne `checked`:**

```
-2147483648
```

**Erklärung:** Ein `int` in C# verwendet 32 Bit im *Zweierkomplement*. Der größte darstellbare Wert ist `2.147.483.647` (alle Bits auf 1, außer dem Vorzeichenbit). Addiert man 1, „kippt" das Vorzeichenbit und der Wert springt auf die kleinste darstellbare Zahl: `int.MinValue` = `-2.147.483.648`. Das nennt man einen **Überlauf** (Overflow) — der Wert läuft quasi „über die Oberkante" und kommt unten wieder heraus.

Standardmäßig wirft C# dabei **keine Fehlermeldung** — der Code läuft still weiter mit einem falschen Ergebnis.

**Mit `checked`:**

```csharp
checked
{
    int max = int.MaxValue;
    int ergebnis = max + 1;  // wirft OverflowException!
    Console.WriteLine(ergebnis);
}
```

Innerhalb eines `checked`-Blocks erkennt C# den Überlauf und wirft eine `OverflowException`. Das Programm bricht ab, statt mit falschen Werten weiterzurechnen.

**Wann ist das in der Praxis gefährlich?**

- **Finanzsoftware:** Ein Kontostand, der durch Überlauf plötzlich negativ wird.
- **Langlebige Zähler:** Ein Zähler, der über Monate/Jahre hochzählt und irgendwann `int.MaxValue` überschreitet.
- **Speicherberechnungen:** Wenn die Größe eines Arrays berechnet wird und das Ergebnis überläuft, wird zu wenig Speicher reserviert — das kann zu Sicherheitslücken führen.

**Reicht `long`?** Der Typ `long` (64 Bit) hat eine Obergrenze von ca. 9,2 Trillionen — das verschiebt das Problem, beseitigt es aber nicht grundsätzlich. Für beliebig große Zahlen bietet C# den Typ `BigInteger` aus `System.Numerics`.

</details>
