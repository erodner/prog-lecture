---
title: "Der Debugger"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal läuft ein Programm, aber das Ergebnis stimmt nicht – und man hat keine Ahnung, warum. Der Debugger ist in solchen Momenten das wichtigste Werkzeug: er erlaubt es, das Programm Schritt für Schritt zu verfolgen und dabei live zu beobachten, was mit den Variablen passiert.

Der Debugger erlaubt es, ein Programm **Schritt für Schritt** auszuführen und dabei den Zustand aller Variablen zu beobachten.

## Breakpoints setzen

Ein **Breakpoint** hält das Programm an einer bestimmten Zeile an. In Visual Studio: Klick auf den grauen Rand links neben der Zeilennummer (roter Punkt erscheint).

```csharp
int a = 5;
int b = 3;
int ergebnis = a * b;   // ← Breakpoint hier setzen
Console.WriteLine(ergebnis);
```

Startet man mit **F5** (Debug-Modus), hält das Programm an der markierten Zeile. In der Variablenansicht sieht man: `a = 5`, `b = 3`, `ergebnis = 0` (noch nicht berechnet).

## Schrittweise ausführen

| Taste | Aktion |
| :--- | :--- |
| **F10** | Step Over – nächste Zeile ausführen |
| **F11** | Step Into – in aufgerufene Methode hineinspringen |
| **Shift+F11** | Step Out – aktuelle Methode verlassen |
| **F5** | Weiter bis zum nächsten Breakpoint |

## Variablen beobachten

Während das Programm pausiert, kann man:
- Mit der Maus über eine Variable **hovern** → Wert wird angezeigt
- Das **Locals**-Fenster zeigt alle lokalen Variablen
- Das **Watch**-Fenster ermöglicht eigene Ausdrücke zu beobachten

## Praktisches Beispiel

```csharp
int summe = 0;
for (int i = 1; i <= 5; i++)
{
    summe = summe + i;  // ← Breakpoint hier
}
Console.WriteLine(summe);
```

Setze einen Breakpoint in der Schleife und drücke wiederholt F10. Beobachte, wie sich `i` und `summe` mit jedem Schleifendurchlauf verändern.

Übung: Schreibe eine Schleife, die die Summe von 1 bis 100 berechnet. Setze einen Breakpoint und überprüfe nach dem 10. Durchlauf, ob `summe` den Wert 55 hat.
{: .notice--info}
