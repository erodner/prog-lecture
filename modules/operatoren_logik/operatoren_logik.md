---
title: "Vergleichs- und Logikoperatoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Ist der Benutzer alt genug? Hat er ein gültiges Ticket? Liegt die Temperatur in einem bestimmten Bereich? Solche Fragen beantwortet man im Code mit Vergleichs- und Logikoperatoren – sie sind das Fundament jeder Entscheidungslogik. Während arithmetische Operatoren Zahlen produzieren, produzieren diese Operatoren immer einen Wahrheitswert: `true` oder `false`.

## Vergleichsoperatoren

Ein Vergleich nimmt zwei Werte und prüft, ob eine bestimmte Beziehung zwischen ihnen gilt. Das Ergebnis ist immer ein `bool` — entweder `true` oder `false`. C# kennt sechs Vergleichsoperatoren:

```csharp
int a = 10, b = 20;

Console.WriteLine(a == b);  // false – gleich
Console.WriteLine(a != b);  // true  – ungleich
Console.WriteLine(a <  b);  // true  – kleiner als
Console.WriteLine(a >  b);  // false – größer als
Console.WriteLine(a <= b);  // true  – kleiner oder gleich
Console.WriteLine(a >= b);  // false – größer oder gleich
```

Besonders wichtig: `==` (Vergleich) ist etwas völlig anderes als `=` (Zuweisung). `a == b` fragt „sind diese Werte gleich?", während `a = b` den Wert von `b` in `a` kopiert. Diese Verwechslung ist ein klassischer Anfängerfehler.
{: .notice--warning}

## Logikoperatoren

Vergleiche allein reichen oft nicht aus — man möchte mehrere Bedingungen kombinieren. Dafür gibt es die drei logischen Operatoren `&&` (UND), `||` (ODER) und `!` (NICHT):

- **`&&` (UND)**: Das Ergebnis ist `true`, wenn **beide** Seiten `true` sind. Sobald eine Seite `false` ist, ist das Gesamtergebnis `false`.
- **`||` (ODER)**: Das Ergebnis ist `true`, wenn **mindestens eine** Seite `true` ist. Nur wenn beide `false` sind, ist das Ergebnis `false`.
- **`!` (NICHT)**: Kehrt einen einzelnen `bool`-Wert um — aus `true` wird `false` und umgekehrt.

```csharp
bool sonnig = true;
bool warm   = false;

Console.WriteLine(sonnig && warm);   // false – UND: beide müssen true sein
Console.WriteLine(sonnig || warm);   // true  – ODER: mindestens einer muss true sein
Console.WriteLine(!sonnig);          // false – NICHT: kehrt den Wert um
```

In der Praxis werden Vergleiche und Logikoperatoren fast immer zusammen verwendet. Ein Vergleich liefert einen `bool`-Wert, und Logikoperatoren verknüpfen diese `bool`-Werte zu komplexeren Bedingungen. 

Im Folgenden greifen wir ein wenig vor und nutzen sogenannte `if`-Anweisungen um die Funktionsweise von Logikausdrücken zu erläutern. Eine `if`-Anweisung stellt Bedingungen für die Ausführung von nachfolgenden Codeanweisungen.
{: .notice--primary}

```csharp
int alter = 20;
bool hatAusweis = true;

// Beide Bedingungen müssen gelten:
if (alter >= 18 && hatAusweis)
    Console.WriteLine("Zutritt erlaubt.");

int note = 2;
// Mindestens eine der Bedingungen muss gelten:
if (note == 1 || note == 2)
    Console.WriteLine("Bestanden mit Auszeichnung.");
```

Man kann beliebig viele Bedingungen verketten. Dabei gilt: `&&` bindet stärker als `||` (genau wie `*` stärker bindet als `+`). Im Zweifel Klammern setzen, um die Absicht deutlich zu machen:

```csharp
// Ohne Klammern schwer zu lesen:
if (alter >= 18 && hatAusweis || istVIP)  // Was wird zuerst ausgewertet?

// Mit Klammern eindeutig:
if ((alter >= 18 && hatAusweis) || istVIP)
```

Wir werden diese Reihenfolge von Operatoren später noch besprechen.

## Kurzschlussauswertung

C# wertet `&&` und `||` von links nach rechts aus und bricht ab, sobald das Ergebnis feststeht. Bei `&&`: Wenn der linke Teil `false` ist, kann das Gesamtergebnis nur `false` sein — der rechte Teil wird gar nicht mehr ausgewertet. Bei `||`: Wenn der linke Teil `true` ist, steht das Ergebnis bereits fest.

```csharp
// Wenn alter < 18 → false, wird DatenbankPrüfung() gar nicht aufgerufen
if (alter >= 18 && DatenbankPrüfung())
{ ... }
```

Das ist nicht nur eine Performance-Optimierung, sondern auch ein Sicherheitsmechanismus. Man kann damit Fehler vermeiden, indem man eine Schutzbedingung voranstellt:

```csharp
// Erst prüfen ob text nicht null ist, dann erst auf Length zugreifen:
if (text != null && text.Length > 0)
    Console.WriteLine(text);
```

Wäre `text` hier `null` und C# würde trotzdem `text.Length` auswerten, gäbe es einen `NullReferenceException`-Laufzeitfehler. Durch die Kurzschlussauswertung passiert das nicht.
Was genau der Wert `null` bedeutet, werden wir später noch besprechen, man kann sich dies wie einen nicht gesetzten Wert vorstellen.

## De Morgansche Gesetze

Beim Vereinfachen oder Umformulieren von Bedingungen sind die De Morganschen Gesetze unverzichtbar. Sie beschreiben, wie Negation und logische Operatoren ineinandergreifen:

```
!(A && B)  ==  !A || !B
!(A || B)  ==  !A && !B
```

Auf Deutsch: „Nicht (A und B)" ist dasselbe wie „Nicht-A oder Nicht-B". Und „Nicht (A oder B)" ist dasselbe wie „Nicht-A und Nicht-B". Die Negation wirkt sich also auf den Operator aus!

Ein praktisches Beispiel: Man möchte prüfen, ob eine Eingabe **nicht** gültig ist. Eine Eingabe gilt als gültig, wenn sie nicht leer und nicht null ist:

```csharp
// Ursprüngliche Bedingung (gültig):
if (eingabe != null && eingabe.Length > 0)
    Verarbeite(eingabe);

// Negiert mit De Morgan (ungültig):
if (eingabe == null || eingabe.Length == 0)
    Console.WriteLine("Ungültige Eingabe.");
```

Die De Morganschen Gesetze helfen auch dabei, verschachtelte Negationen zu entfernen, die Code schwer lesbar machen:

```csharp
// Schwer lesbar:
if (!(alter < 18 || !hatTicket))
    { ... }

// Nach De Morgan: !(A || B) == !A && !B, und !!hatTicket == hatTicket
if (alter >= 18 && hatTicket)
    { ... }
```

Die Regel lässt sich merken als: **Negation reinziehen → Operatoren umkehren** (`&&` wird `||` und umgekehrt).
{: .notice--primary}

Übung: Schreibe eine Bedingung, die prüft ob eine Zahl zwischen 1 und 100 liegt (inklusive). Formuliere sie dann mit De Morgan so um, dass sie prüft ob die Zahl **außerhalb** dieses Bereichs liegt.
{: .notice--info}

## Weitere Quellen

- [Boolesche Logikoperatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/boolean-logical-operators)
- [Vergleichsoperatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/comparison-operators)
