---
title: "🧩 Aufgaben und Beispiele: Methoden"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmieren lernt man nicht nur durch Codezeilen tippen — sondern auch durch **Nachdenken**. Die folgenden Aufgaben trainieren *Computational Thinking*: die Fähigkeit, Probleme so zu strukturieren, dass ein Computer sie lösen kann. Dazu gehören Abstraktion, Zerlegung, Mustererkennung und Algorithmenentwurf. Nimm dir für jede Aufgabe Zeit, bevor du die Lösung aufklappst.

## Aufgabe 1 — Zerlegung (schwierig)

Ein Programm soll die Endnote eines Studiengangs berechnen. Die Regeln:
- Jedes Modul hat eine Note (1.0–5.0) und eine Gewichtung in ECTS-Punkten
- Der Durchschnitt wird ECTS-gewichtet berechnet: `Summe(Note × ECTS) / Summe(ECTS)`
- Module mit Note 5.0 gelten als nicht bestanden
- Der Studiengang ist bestanden, wenn alle Module bestanden sind **UND** der Gesamtdurchschnitt ≤ 4.0 ist

Zerlege dieses Problem in sinnvolle Methoden. Entwirf nur die **Signaturen** (Name, Parameter, Rückgabetyp) — noch keinen Methodenrumpf. Überlege:
- Welche Teilprobleme gibt es?
- Welche Methode ruft welche andere auf?
- Welche Daten werden als Parameter übergeben?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Teilprobleme identifizieren:**

Das Gesamtproblem „Studiengang bestanden?" besteht aus drei klar abgrenzbaren Teilproblemen:

1. **ECTS-gewichteten Durchschnitt berechnen** — reine Mathematik
2. **Prüfen, ob alle Module bestanden sind** — jede Note einzeln prüfen
3. **Gesamtentscheidung treffen** — die beiden Teilergebnisse kombinieren

**Schritt 2 — Signaturen entwerfen:**

```csharp
// Teilproblem 1: Gewichteten Durchschnitt berechnen
static double GewichteterDurchschnitt(double[] noten, int[] ects)

// Teilproblem 2: Sind alle Module bestanden (keine Note 5.0)?
static bool AlleModuleBestanden(double[] noten)

// Gesamtproblem: Studiengang bestanden?
static bool StudiengangBestanden(double[] noten, int[] ects)
```

**Schritt 3 — Aufrufstruktur:**

`StudiengangBestanden` ruft die beiden anderen Methoden auf und kombiniert deren Ergebnisse. Jede Methode löst genau **ein** Teilproblem — das ist das Prinzip der Zerlegung (*Decomposition*).

```
StudiengangBestanden
├── AlleModuleBestanden
└── GewichteterDurchschnitt
```

**Schritt 4 — Implementierung:**

```csharp
static double GewichteterDurchschnitt(double[] noten, int[] ects)
{
    double summeNotenMalEcts = 0;
    int summeEcts = 0;

    for (int i = 0; i < noten.Length; i++)
    {
        summeNotenMalEcts += noten[i] * ects[i];
        summeEcts += ects[i];
    }

    return summeNotenMalEcts / summeEcts;
}

static bool AlleModuleBestanden(double[] noten)
{
    for (int i = 0; i < noten.Length; i++)
    {
        if (noten[i] >= 5.0)
        {
            return false;
        }
    }
    return true;
}

static bool StudiengangBestanden(double[] noten, int[] ects)
{
    return AlleModuleBestanden(noten)
        && GewichteterDurchschnitt(noten, ects) <= 4.0;
}
```

Beachte: `StudiengangBestanden` enthält selbst fast keine Logik — sie **delegiert** an die Teilmethoden und verknüpft nur deren Ergebnisse. Das macht den Code lesbar und jede Methode einzeln testbar.

</details>
