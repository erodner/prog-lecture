---
title: "break und continue"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal will man eine Schleife vorzeitig abbrechen – etwa wenn man das gesuchte Element gefunden hat. Oder man möchte einen einzelnen Durchlauf überspringen, ohne die ganze Schleife zu beenden. Dafür gibt es `break` und `continue`.

## `break` – Schleife sofort beenden

`break` bricht die Schleife ab, unabhängig von der Schleifenbedingung.

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i == 5)
        break;             // Schleife bei i=5 abbrechen
    Console.Write(i + " ");
}
// Ausgabe: 1 2 3 4
```

**Typischer Einsatz:** Suche in einer Liste – sobald das gesuchte Element gefunden wurde, kann abgebrochen werden.

```csharp
int[] zahlen = { 4, 7, 2, 9, 1, 6 };
int gesucht = 9;
bool gefunden = false;

foreach (int z in zahlen)
{
    if (z == gesucht)
    {
        gefunden = true;
        break;
    }
}
Console.WriteLine(gefunden ? "Gefunden!" : "Nicht gefunden.");
```

## `continue` – aktuellen Durchlauf überspringen

`continue` springt direkt zum nächsten Schleifendurchlauf.

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i % 2 == 0)
        continue;          // gerade Zahlen überspringen
    Console.Write(i + " ");
}
// Ausgabe: 1 3 5 7 9
```

## Übersicht

| Anweisung | Wirkung |
| :--- | :--- |
| `break` | Verlässt die Schleife sofort |
| `continue` | Überspringt den Rest des aktuellen Durchlaufs |

Übermäßiger Einsatz von `break` und `continue` kann Code schwer lesbar machen. Im Zweifel lieber die Schleifenbedingung anpassen.
{: .notice--primary}
