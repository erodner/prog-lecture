---
title: "🧩 Aufgaben und Beispiele: Arrays"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmieren lernt man nicht nur durch Codezeilen tippen — sondern auch durch **Nachdenken**. Die folgenden Aufgaben trainieren *Computational Thinking*: die Fähigkeit, Probleme so zu strukturieren, dass ein Computer sie lösen kann. Dazu gehören Abstraktion, Zerlegung, Mustererkennung und Algorithmenentwurf. Nimm dir für jede Aufgabe Zeit, bevor du die Lösung aufklappst.

## Aufgabe 1 — Array-Rotation

Rotiere ein Array um k Positionen nach links. Jedes Element rückt um k Plätze nach vorne, Elemente die „herausfallen" erscheinen am Ende wieder.

```
[1, 2, 3, 4, 5] mit k=2  →  [3, 4, 5, 1, 2]
[10, 20, 30, 40] mit k=1  →  [20, 30, 40, 10]
```

Löse die Aufgabe mit einem Hilfs-Array. Überlege dann: Welche Formel berechnet die neue Position eines Elements?

**Bonusfrage:** Was passiert wenn k größer als die Arraylänge ist? Wie gehst du damit um?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Die Kernidee:**

Bei einer Linksrotation um k wandert das Element an Position `i` im alten Array zur Position `(i - k + n) % n` im neuen Array. Äquivalent ausgedrückt: Die neue Position `i` im Ergebnis-Array wird mit dem Element an Position `(i + k) % n` im Original befüllt. Der Modulo-Operator `%` sorgt dafür, dass der Index „umspringt".

**Schritt 2 — C#-Code mit Hilfs-Array:**

```csharp
int[] arr = { 1, 2, 3, 4, 5 };
int k = 2;
int n = arr.Length;

// k normalisieren, falls k >= n
k = k % n;

int[] ergebnis = new int[n];

for (int i = 0; i < n; i++)
{
    ergebnis[i] = arr[(i + k) % n];
}

// Ergebnis ausgeben
foreach (int x in ergebnis)
{
    Console.Write(x + " ");
}
Console.WriteLine();
// Ausgabe: 3 4 5 1 2
```

**Warum funktioniert `(i + k) % n`?** Für Position `i=0` im Ergebnis wollen wir das Element, das im Original an Position `k` steht. Für `i=1` das Element an Position `k+1`, usw. Wenn der Index über das Ende hinausgeht, springt `% n` zurück zum Anfang. Beispiel mit k=2, n=5: Position 3 im Ergebnis bekommt `arr[(3+2) % 5] = arr[0] = 1` — genau richtig!

**Bonus — Wenn k größer als die Arraylänge ist:**

Eine Rotation um k=7 bei einem Array der Länge 5 ist dasselbe wie eine Rotation um k=2, denn nach 5 Rotationen ist das Array wieder im Ursprungszustand. Deshalb normalisieren wir mit `k = k % n` am Anfang. Das behandelt auch den Fall k=0 (keine Rotation) korrekt.

</details>

## Aufgabe 2 — Algorithmenentwurf (schwieriger)

Gegeben sind zwei **bereits sortierte** Arrays. Erstelle ein neues Array, das alle Elemente aus beiden Arrays enthält — und zwar ebenfalls sortiert. Nutze dabei aus, dass die Eingabe-Arrays bereits sortiert sind (nicht einfach zusammenkopieren und dann sortieren!).

```
[1, 3, 5, 7] + [2, 4, 6, 8] → [1, 2, 3, 4, 5, 6, 7, 8]
[1, 2, 5]    + [3, 4]       → [1, 2, 3, 4, 5]
```

**Tipp:** Stelle dir vor, du hast zwei Stapel sortierter Karten vor dir. Wie würdest du sie zu einem sortierten Stapel zusammenlegen?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Idee (Zwei-Zeiger-Verfahren):**

Wir verwenden je einen Zeiger (Index) für jedes Array. In jedem Schritt vergleichen wir die Elemente an den beiden Zeigerpositionen und nehmen das kleinere in das Ergebnis-Array. Der entsprechende Zeiger rückt um eins vor. Wenn ein Array aufgebraucht ist, kopieren wir den Rest des anderen Arrays.

**Schritt 2 — Pseudocode:**

```
Zeiger i auf Array A (startet bei 0)
Zeiger j auf Array B (startet bei 0)
Zeiger k auf Ergebnis-Array (startet bei 0)

Solange i < Länge von A UND j < Länge von B:
    Falls A[i] <= B[j]:
        Ergebnis[k] = A[i], i vorrücken
    Sonst:
        Ergebnis[k] = B[j], j vorrücken
    k vorrücken

Restliche Elemente von A kopieren (falls vorhanden)
Restliche Elemente von B kopieren (falls vorhanden)
```

**Schritt 3 — C#-Code:**

```csharp
int[] a = { 1, 3, 5, 7 };
int[] b = { 2, 4, 6, 8 };
int[] ergebnis = new int[a.Length + b.Length];

int i = 0; // Zeiger für Array a
int j = 0; // Zeiger für Array b
int k = 0; // Zeiger für Ergebnis-Array

// Hauptschleife: Vergleiche und nimm das kleinere Element
while (i < a.Length && j < b.Length)
{
    if (a[i] <= b[j])
    {
        ergebnis[k] = a[i];
        i++;
    }
    else
    {
        ergebnis[k] = b[j];
        j++;
    }
    k++;
}

// Rest von a kopieren (falls noch Elemente übrig)
while (i < a.Length)
{
    ergebnis[k] = a[i];
    i++;
    k++;
}

// Rest von b kopieren (falls noch Elemente übrig)
while (j < b.Length)
{
    ergebnis[k] = b[j];
    j++;
    k++;
}

// Ergebnis ausgeben
foreach (int x in ergebnis)
{
    Console.Write(x + " ");
}
Console.WriteLine();
```

**Warum ist das clever?** Wir gehen jedes Element genau einmal durch — das sind insgesamt `a.Length + b.Length` Schritte. Würden wir stattdessen beide Arrays zusammenkopieren und dann sortieren, bräuchte der Sortieralgorithmus deutlich mehr Schritte. Dieses Verfahren ist übrigens der **Merge-Schritt** des berühmten *Merge Sort*-Algorithmus.

</details>
