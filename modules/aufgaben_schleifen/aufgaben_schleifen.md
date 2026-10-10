---
title: "🧩 Aufgaben und Beispiele: Schleifen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmieren lernt man nicht nur durch Codezeilen tippen — sondern auch durch **Nachdenken**. Die folgenden Aufgaben trainieren *Computational Thinking*: die Fähigkeit, Probleme so zu strukturieren, dass ein Computer sie lösen kann. Dazu gehören Abstraktion, Zerlegung, Mustererkennung und Algorithmenentwurf. Nimm dir für jede Aufgabe Zeit, bevor du die Lösung aufklappst.

## Aufgabe 1 — Collatz-Folge (das 3n+1-Problem)

Die Collatz-Folge beginnt mit einer beliebigen positiven Ganzzahl und folgt zwei einfachen Regeln: Ist die Zahl gerade, halbiere sie. Ist sie ungerade, nimm 3n + 1. Die Vermutung: Egal mit welcher Zahl man startet, man landet irgendwann bei 1.

```
Beispiel für n = 6:  6 → 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1  (8 Schritte)
Beispiel für n = 27: 27 → 82 → 41 → ... → 1  (111 Schritte!)
```

Schreibe ein Programm, das für eine Startzahl die Collatz-Folge ausgibt und die Anzahl der Schritte zählt. Teste mit n=27 — wie viele Schritte braucht es?

**Bonusfrage:** Welche Startzahl unter 100 braucht die meisten Schritte?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Algorithmus verstehen:**

Wir brauchen eine `while`-Schleife, die so lange läuft, bis `n` den Wert 1 erreicht. In jedem Schritt prüfen wir: Ist `n` gerade (`n % 2 == 0`)? Dann halbieren. Sonst rechnen wir `3 * n + 1`. Dabei zählen wir die Schritte mit.

**Schritt 2 — C#-Code:**

```csharp
int n = 27;
int schritte = 0;
int maximum = n;

Console.Write(n);

while (n != 1)
{
    if (n % 2 == 0)
    {
        n = n / 2;
    }
    else
    {
        n = 3 * n + 1;
    }

    if (n > maximum)
    {
        maximum = n;
    }

    schritte++;
    Console.Write(" → " + n);
}

Console.WriteLine();
Console.WriteLine($"Schritte: {schritte}");
Console.WriteLine($"Maximum erreicht: {maximum}");
```

**Ergebnis für n=27:** Die Folge braucht **111 Schritte** und erreicht dabei ein Maximum von **9232**, bevor sie wieder auf 1 herunterkommt. Das ist erstaunlich viel für eine so kleine Startzahl!

**Bonus — Startzahl mit den meisten Schritten unter 100:**

```csharp
int meistStart = 1;
int meistSchritte = 0;

for (int start = 2; start < 100; start++)
{
    int n = start;
    int schritte = 0;

    while (n != 1)
    {
        if (n % 2 == 0)
        {
            n = n / 2;
        }
        else
        {
            n = 3 * n + 1;
        }
        schritte++;
    }

    if (schritte > meistSchritte)
    {
        meistSchritte = schritte;
        meistStart = start;
    }
}

Console.WriteLine($"Startzahl mit den meisten Schritten: {meistStart} ({meistSchritte} Schritte)");
```

Die Antwort ist **n=97** mit **118 Schritten** — das ist die Startzahl unter 100, die am längsten braucht.

**Wusstest du?** Die Collatz-Vermutung ist ein **ungelöstes Problem der Mathematik**! Niemand konnte bisher beweisen, dass die Folge tatsächlich für *jede* Startzahl bei 1 endet. Der Mathematiker Paul Erdős sagte dazu: „Die Mathematik ist noch nicht reif genug für solche Probleme."

</details>

## Aufgabe 2 — Primzahltest

Schreibe ein Programm, das prüft ob eine Zahl eine Primzahl ist. Eine Primzahl ist nur durch 1 und sich selbst teilbar.

**Naiver Ansatz:** Teste alle Teiler von 2 bis n-1. **Optimierter Ansatz:** Warum reicht es, nur bis √n zu testen? Überlege mathematisch, bevor du den Code schreibst.

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Naiver Ansatz:**

Wir testen alle möglichen Teiler von 2 bis n-1. Sobald einer `n` ohne Rest teilt, ist `n` keine Primzahl.

```csharp
int n = 29;
bool istPrimzahl = true;

if (n < 2)
{
    istPrimzahl = false;
}
else
{
    for (int i = 2; i < n; i++)
    {
        if (n % i == 0)
        {
            istPrimzahl = false;
            break;
        }
    }
}

Console.WriteLine($"{n} ist {(istPrimzahl ? "eine Primzahl" : "keine Primzahl")}");
```

Das funktioniert, ist aber langsam für große Zahlen. Für n = 1.000.000.007 müssten wir über eine Milliarde Teiler durchprobieren!

**Schritt 2 — Die √n-Optimierung:**

Die zentrale Einsicht: Wenn `n = a × b`, dann muss **mindestens einer** der beiden Faktoren kleiner oder gleich √n sein. Denn wären beide größer als √n, wäre ihr Produkt größer als n — Widerspruch! Wenn wir also bis √n keinen Teiler gefunden haben, gibt es keinen.

**Schritt 3 — Optimierter C#-Code:**

```csharp
int n = 1000000007;
bool istPrimzahl = true;

if (n < 2)
{
    istPrimzahl = false;
}
else
{
    for (int i = 2; i * i <= n; i++)
    {
        if (n % i == 0)
        {
            istPrimzahl = false;
            break;
        }
    }
}

Console.WriteLine($"{n} ist {(istPrimzahl ? "eine Primzahl" : "keine Primzahl")}");
```

**Warum `i * i <= n` statt `i <= Math.Sqrt(n)`?** Die Bedingung `i * i <= n` vermeidet die Berechnung einer Gleitkomma-Wurzel und arbeitet rein mit ganzen Zahlen — das ist präziser und schneller. Beide Varianten sind äquivalent, aber bei ganzzahliger Arithmetik gibt es keine Rundungsfehler.

**Sonderfälle nicht vergessen:** 0 und 1 sind **keine** Primzahlen. Die 2 ist die kleinste Primzahl und die einzige gerade.

</details>

## Aufgabe 3 — Quersumme beliebig langer Zahlen

In den [Operator-Aufgaben](/modules/aufgaben_operatoren/aufgaben_operatoren.md) haben wir die Quersumme einer vierstelligen Zahl berechnet, indem wir viermal dieselbe Zeile `summe += zahl % 10; zahl /= 10;` geschrieben haben. Schreibe jetzt ein Programm, das die Quersumme einer **beliebig langen** positiven Ganzzahl berechnet.

**Beispiele:** Quersumme von 427 = 13, Quersumme von 9999 = 36, Quersumme von 1000000 = 1.

Welche Schleifenart passt hier — und warum nicht `for`?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Idee:** Die wiederholte Zeile aus der Operator-Aufgabe wandert in den Schleifenrumpf. Die Abbruchbedingung liefert die Zahl selbst: Sobald `zahl` den Wert 0 erreicht, sind alle Ziffern verarbeitet.

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

**Warum `while` statt `for`?** Die Anzahl der Durchläufe hängt von der Stellenzahl ab, die wir vorher nicht kennen. `for` eignet sich, wenn man die Anzahl der Wiederholungen im Voraus weiß; `while`, wenn eine Bedingung über das Ende entscheidet. Die Schleife hat keine fest codierte Anzahl an Durchläufen — egal ob die Zahl 3 oder 10 Stellen hat, der Algorithmus funktioniert identisch.

**Sonderfall:** Für `zahl = 0` wird die Schleife gar nicht betreten und `summe` bleibt 0 — das ist auch die korrekte Quersumme.

</details>
