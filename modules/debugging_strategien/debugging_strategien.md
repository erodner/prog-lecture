---
title: "Debugging-Strategien und Testen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Den Debugger in Visual Studio zu kennen ist gut — aber Debugging ist mehr als ein Werkzeug. Es ist eine Denkweise: systematisch eingrenzen, statt ziellos im Code herumzuändern. Und wer seine Programme von Anfang an mit guten Testfällen prüft, findet Fehler, bevor sie zu Problemen werden.

## Strategie 1: Console.WriteLine-Debugging

Die einfachste und oft schnellste Methode: Zwischenwerte ausgeben, um zu sehen was das Programm tatsächlich tut.

```csharp
double a = 10, b = 20, c = 30;
double durchschnitt = a + b + c / 3;

Console.WriteLine($"DEBUG: a={a}, b={b}, c={c}");
Console.WriteLine($"DEBUG: c/3 = {c / 3}");
Console.WriteLine($"DEBUG: durchschnitt = {durchschnitt}");
// Ausgabe zeigt: durchschnitt = 40 → c/3 wird zuerst berechnet!
```

Diese Technik funktioniert überall — auch wenn kein Debugger verfügbar ist. Die `DEBUG:`-Zeilen nach dem Fixen einfach wieder entfernen.

## Strategie 2: Problem eingrenzen

Wenn unklar ist, **wo** der Fehler liegt:

1. **Halbieren**: Die Hälfte des Codes auskommentieren. Tritt der Fehler noch auf? Wenn ja, liegt er in der verbleibenden Hälfte. Wenn nein, in der auskommentierten. Dann die betroffene Hälfte wieder halbieren — so findet man den Fehler in wenigen Schritten.

2. **Minimales Beispiel bauen**: Den Code auf das Nötigste reduzieren, bis der Fehler verschwindet. Die letzte Änderung, die ihn wieder auslöst, ist der Schuldige.

## Strategie 3: Rubber Duck Debugging

Das Problem **laut erklären** — einem Kommilitonen, einem Gummientchen oder sich selbst. Dabei fällt oft auf, dass man eine Annahme macht, die nicht stimmt:

*"Also, ich lese eine Zahl ein, teile sie durch 3, und... Moment, ich teile ja einen `int` durch einen `int`, da kommt eine Ganzzahldivision raus!"*

Der Trick funktioniert, weil Erklären ein anderes Denken erzwingt als Lesen. Man muss jeden Schritt begründen — und genau dabei stolpert man über den Fehler.

Rubber Duck Debugging klingt albern, ist aber eine der effektivsten Debugging-Methoden. Viele professionelle Entwickler schwören darauf.
{: .notice--primary}

## Strategie 4: Der Debugger

Der Debugger in Visual Studio erlaubt es, das Programm Zeile für Zeile auszuführen und dabei den Zustand aller Variablen zu beobachten:

- **Breakpoint setzen** (F9 oder Klick auf den linken Rand): Das Programm hält an dieser Stelle an
- **Einzelschritt** (F10): Eine Zeile ausführen und anhalten
- **Hineinspringen** (F11): In einen Methodenaufruf hineinspringen
- **Variablen inspizieren**: Maus über eine Variable halten oder das Fenster "Lokale Variablen" nutzen

Der Debugger ist besonders nützlich bei Logikfehlern, bei denen das Programm nicht abstürzt, aber falsche Ergebnisse liefert — man sieht genau, an welcher Stelle der Wert vom Erwarteten abweicht.

## Programme testen: Woher weiß ich, dass es richtig ist?

"Es läuft ohne Fehler" bedeutet nicht "es ist korrekt". Ein Programm, das für eine Eingabe das richtige Ergebnis liefert, kann bei einer anderen Eingabe scheitern. Gutes Testen heißt: sich gezielt Eingaben überlegen, die Schwächen aufdecken.

### Testfälle vor dem Programmieren überlegen

Bevor man Code schreibt, lohnt es sich zu fragen: *Welche Eingaben sind typisch? Welche sind ungewöhnlich? Was sind die Grenzen?*

Beispiel — ein Programm soll prüfen, ob eine Zahl positiv ist:

| Eingabe | Erwartetes Ergebnis | Warum dieser Test? |
| :--- | :--- | :--- |
| `5` | positiv | Normalfall |
| `-3` | nicht positiv | Negativer Wert |
| `0` | nicht positiv | Grenzfall — ist 0 positiv? |
| `2147483647` | positiv | Größter `int`-Wert |

### Grenzwerte und Sonderfälle

Die meisten Fehler treten nicht bei "normalen" Eingaben auf, sondern an den Rändern:

- **Null und leere Werte**: Was passiert bei `0`, bei `""`, bei `null`?
- **Grenzen von Zahlenbereichen**: Was passiert bei `int.MaxValue` oder `int.MinValue`?
- **Ungültige Eingaben**: Was passiert, wenn der Benutzer einen Buchstaben statt einer Zahl eingibt?
- **Einelementige und leere Fälle**: Funktioniert der Durchschnitt bei nur einer Zahl? Bei keiner?

### Erwartetes vs. tatsächliches Ergebnis

Beim Testen immer **vorher** aufschreiben, was herauskommen soll — erst dann ausführen und vergleichen:

```csharp
// Test: Durchschnitt von 10, 20, 30 soll 20.0 sein
double ergebnis = (10 + 20 + 30) / 3.0;
Console.WriteLine($"Ergebnis: {ergebnis}, Erwartet: 20.0");
```

Wenn man erst das Ergebnis sieht und dann entscheidet ob es "richtig aussieht", übersieht man leicht subtile Fehler.

Teste immer zuerst die einfachsten Fälle. Wenn die scheitern, ist der Fehler leichter zu finden als bei einem komplexen Testfall.
{: .notice--primary}

Übung: Schreibe ein Programm, das eine Konsoleneingabe in eine ganze Zahl umwandelt und ausgibt, ob sie gerade oder ungerade ist. Überlege dir **vorher** mindestens 5 Testfälle (inklusive Grenzwerte und ungültige Eingaben) und prüfe dein Programm damit.
{: .notice--info}

## Weitere Quellen

- [Debugger-Einführung in Visual Studio – Microsoft Learn](https://learn.microsoft.com/de-de/visualstudio/debugger/debugger-feature-tour)
- [Debugging für Anfänger – Microsoft Learn](https://learn.microsoft.com/de-de/visualstudio/debugger/debugging-absolute-beginners)
- [Rubber Duck Debugging (Erklärung und Hintergrund)](https://rubberduckdebugging.com/)
