---
title: "🧩 Aufgaben und Beispiele: Bedingte Anweisungen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bedingte Anweisungen sind das Herzstück jeder Programmlogik. Doch die richtige Struktur zu finden, erfordert Übung im *Computational Thinking* — vor allem in der **Zerlegung** komplexer Regeln in klare, geordnete Bedingungen. Die folgenden Aufgaben trainieren genau das.

## Aufgabe 1 — De Morgansche Gesetze

Die folgenden Bedingungen lassen sich jeweils vereinfachen. Forme den Ausdruck auf der linken Seite um, **ohne ihn auszuführen** — nur durch logisches Nachdenken:

```
1.  !(alter >= 18 && alter <= 65)   →  ???
2.  !(istStudent || hatAusweis)     →  ???
3.  !(x > 0 && y > 0 && z > 0)     →  ???
```

Welche allgemeine Regel steckt dahinter? Formuliere sie in eigenen Worten.

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Umformungen:**

```
1.  !(alter >= 18 && alter <= 65)   →  alter < 18 || alter > 65
2.  !(istStudent || hatAusweis)     →  !istStudent && !hatAusweis
3.  !(x > 0 && y > 0 && z > 0)     →  x <= 0 || y <= 0 || z <= 0
```

**Was passiert hier?** Wir wenden die **De Morganschen Gesetze** an (benannt nach Augustus De Morgan, einem britischen Mathematiker des 19. Jahrhunderts):

- `!(A && B)` ist dasselbe wie `!A || !B`
- `!(A || B)` ist dasselbe wie `!A && !B`

In Worten: **Wenn du ein NOT über eine Klammer ziehst, drehen sich alle Vergleiche um und AND wird zu OR (und umgekehrt).**

**Schritt-für-Schritt für Beispiel 1:**

Die Bedingung `!(alter >= 18 && alter <= 65)` bedeutet: „Es stimmt NICHT, dass das Alter zwischen 18 und 65 liegt." Das NOT verteilt sich auf beide Teile:

- `alter >= 18` wird zu `alter < 18` (Vergleich dreht sich um)
- `&&` wird zu `||`
- `alter <= 65` wird zu `alter > 65` (Vergleich dreht sich um)

Ergebnis: `alter < 18 || alter > 65` — also „jünger als 18 ODER älter als 65". Das ist intuitiv korrekt: Wer nicht in der Spanne 18–65 liegt, ist entweder darunter oder darüber.

**Warum ist das praktisch relevant?** Vereinfachte Bedingungen sind leichter zu lesen und weniger fehleranfällig. Statt `!(a && b)` schreibt man lieber `!a || !b` — das vermeidet doppelte Verneinungen, die beim Lesen leicht zu Denkfehlern führen.

</details>

## Aufgabe 2 — Äquivalente Bedingungen erkennen

Welche der folgenden Code-Paare sind **logisch äquivalent**? Bei welchen gibt es einen Unterschied?

**Paar A:**

```csharp
// Version 1:                       // Version 2:
if (x > 0)                         if (x > 0 && y > 0)
    if (y > 0)                         Console.WriteLine("Beide positiv");
        Console.WriteLine("Beide positiv");
```

**Paar B:**

```csharp
// Version 1:                       // Version 2:
if (punkte >= 90)                   if (punkte >= 90)
    note = "sehr gut";                  note = "sehr gut";
else if (punkte >= 75)              if (punkte >= 75)
    note = "gut";                       note = "gut";
```

**Paar C:**

```csharp
// Version 1:                       // Version 2:
bool erlaubt = (alter >= 18)        bool erlaubt = alter >= 18;
    ? true : false;
```

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Paar A — Äquivalent!**

Verschachtelte `if`-Anweisungen ohne `else` sind identisch mit einer einzigen `if`-Anweisung, die beide Bedingungen mit `&&` verknüpft. Die innere Bedingung `y > 0` wird nur geprüft, wenn die äußere `x > 0` wahr ist — und genau das macht auch `&&` durch die sogenannte *Short-Circuit-Evaluation* (Kurzschlussauswertung).

Version 2 ist in der Praxis bevorzugt, weil sie kompakter und leichter zu lesen ist.

**Paar B — NICHT äquivalent!**

Das ist eine wichtige Falle. Testen wir mit `punkte = 95`:

- **Version 1 (else-if):** `95 >= 90` ist wahr → `note = "sehr gut"`. Der `else if`-Zweig wird **übersprungen**. Ergebnis: `"sehr gut"`.
- **Version 2 (separate ifs):** `95 >= 90` ist wahr → `note = "sehr gut"`. Dann wird das **zweite `if`** ebenfalls geprüft: `95 >= 75` ist auch wahr → `note = "gut"`. Die Variable wird **überschrieben**! Ergebnis: `"gut"`.

Der entscheidende Unterschied: `else if` bildet eine **exklusive Kette** — nur der erste zutreffende Zweig wird ausgeführt. Zwei separate `if`-Anweisungen sind dagegen **unabhängig** voneinander. Jede wird einzeln geprüft, und spätere können frühere Ergebnisse überschreiben.

**Paar C — Äquivalent, aber Version 2 ist klar besser!**

Der Ausdruck `alter >= 18` liefert bereits einen `bool`-Wert (`true` oder `false`). Der ternäre Operator `? true : false` ist daher **vollkommen überflüssig** — er wandelt `true` in `true` und `false` in `false` um, also ein Nicht-Operation.

```csharp
// Schlecht (redundant):
bool erlaubt = (alter >= 18) ? true : false;

// Gut (direkt):
bool erlaubt = alter >= 18;
```

Das Muster `bedingung ? true : false` ist ein häufiger **Anfänger-Antipattern**. Man erkennt es daran, dass man sich fragt: „Was passiert, wenn die Bedingung wahr ist? Dann ist das Ergebnis wahr." — das ist eine Tautologie. Wer dieses Muster im eigenen Code findet, kann es immer durch die Bedingung allein ersetzen.

</details>
