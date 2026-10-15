---
title: "Parameter mit ref und out"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bei den [Gültigkeitsbereichen](/modules/scope/scope.md) haben wir gesehen, dass eine Methode standardmäßig nur eine Kopie ihrer Argumente bekommt (Call by Value). Das ist sicher und vorhersagbar, hat aber auch eine Grenze: Eine Methode `Tausche(int x, int y)` kann die Variablen des Aufrufers nicht vertauschen, weil sie nur mit Kopien arbeitet. Als Ausweg haben wir Tupel als Rückgabewert verwendet. C# bietet daneben zwei Schlüsselwörter, mit denen eine Methode tatsächlich auf die Variablen des Aufrufers zugreifen kann: `ref` und `out`.

## Call by Reference mit `ref`

Mit `ref` bekommt die Methode keine Kopie, sondern einen Verweis auf die originale Variable. Jede Zuweisung an den Parameter wirkt direkt auf die Variable des Aufrufers:

```csharp
static void Verdoppeln(ref int x)
{
    x *= 2;
}

int zahl = 10;
Verdoppeln(ref zahl);
Console.WriteLine(zahl); // 20 – geändert!
```

Ohne `ref` hätte `Verdoppeln` nur die eigene Kopie verdoppelt, und `zahl` wäre `10` geblieben. Das `ref`-Schlüsselwort muss sowohl in der Methodensignatur als auch beim Aufruf stehen. Das ist bewusst so, damit es keine versteckten Seiteneffekte gibt: Wer `ref` im Aufruf sieht, weiß sofort, dass diese Variable danach einen anderen Wert haben kann.

Damit lässt sich jetzt auch das Tauschproblem lösen:

```csharp
static void Tausche(ref int x, ref int y)
{
    int tmp = x;
    x = y;
    y = tmp;
}

int a = 1, b = 2;
Tausche(ref a, ref b);
Console.WriteLine($"a = {a}, b = {b}"); // a = 2, b = 1
```

`ref` sollte sparsam eingesetzt werden. Meist ist es klarer, einen Wert per `return` zurückzugeben, statt eine Variable von außen zu verändern. Im Zweifel `return` statt `ref`.
{: .notice--primary}

## `ref` und Arrays

Bei [Werttypen und Referenztypen](/modules/werttypen_referenztypen/werttypen_referenztypen.md) haben wir gesehen, dass eine Methode den *Inhalt* eines übergebenen Arrays auch ohne `ref` verändern kann, weil die kopierte Referenz auf dieselben Daten zeigt. Wozu also `ref` bei Arrays? Der Unterschied zeigt sich, wenn die Methode dem Parameter ein *neues* Array zuweist:

```csharp
static void Verlängern(int[] werte)
{
    werte = new int[werte.Length * 2];  // nur die lokale Kopie der Referenz zeigt jetzt woanders hin
}

static void VerlängernMitRef(ref int[] werte)
{
    werte = new int[werte.Length * 2];  // die Variable des Aufrufers zeigt jetzt auf das neue Array
}

int[] daten = { 1, 2, 3 };
Verlängern(daten);
Console.WriteLine(daten.Length); // 3
VerlängernMitRef(ref daten);
Console.WriteLine(daten.Length); // 6
```

Ohne `ref` kann die Methode also verändern, was *im* Array steht. Mit `ref` kann sie zusätzlich festlegen, *auf welches* Array die Variable des Aufrufers zeigt. Genau so arbeitet übrigens `Array.Resize`, das intern `ref` verwendet.

## Ausgabe-Parameter mit `out`

`out` ist eng mit `ref` verwandt, dient aber einem anderen Zweck: Die Methode *liefert* über den Parameter ein Ergebnis, statt einen vorhandenen Wert zu verändern. Deshalb muss die Variable vorher nicht initialisiert sein, dafür ist die Methode verpflichtet, ihr einen Wert zuzuweisen. Unser Beispiel aus den Gültigkeitsbereichen, das wir dort mit einem Tupel gelöst haben, sieht mit `out` so aus:

```csharp
static void InStundenUndMinuten(int gesamtMinuten, out int stunden, out int minuten)
{
    stunden = gesamtMinuten / 60;
    minuten = gesamtMinuten % 60;
}

InStundenUndMinuten(135, out int h, out int m);
Console.WriteLine($"{h} h {m} min"); // 2 h 15 min
```

Im Vergleich zur Tupel-Variante wirkt das umständlicher: Die Ergebnisse stehen nicht links vom `=`, sondern verstecken sich in der Argumentliste. Für eigene Methoden mit mehreren Ergebnissen sind Tupel deshalb meist die bessere Wahl.

## Das `TryParse`-Muster

Wo `out` seine Stärke ausspielt, ist ein Muster aus der Standardbibliothek, das wir bei der [Typkonvertierung](/modules/typkonvertierung/typkonvertierung.md) schon benutzt haben: `int.TryParse` liefert per `return`, *ob* die Umwandlung geklappt hat, und per `out` das eigentliche Ergebnis. Dadurch lässt sich der Aufruf direkt in eine Bedingung schreiben. Zusammen mit einer [Schleife](/modules/schleifen/schleifen.md) entsteht so eine robuste Eingabe, die so lange nachfragt, bis eine gültige Zahl eingegeben wurde:

```csharp
static int LiesGanzzahl(string frage)
{
    int zahl;
    Console.Write(frage);
    while (!int.TryParse(Console.ReadLine(), out zahl))
    {
        Console.Write("Das war keine gültige Zahl. Bitte nochmal: ");
    }
    return zahl;
}

int alter = LiesGanzzahl("Wie alt bist du? ");
```

Anders als `int.Parse` aus der [Eingabe](/modules/eingabe/eingabe.md) bricht dieses Programm bei einer ungültigen Eingabe nicht ab. Die Methode `LiesGanzzahl` versteckt die ganze Prüflogik, und der Aufrufer bekommt garantiert eine Zahl zurück.

Übung: Schreibe eine Methode `SortiereVariablen(ref int a, ref int b, ref int c)`, die die drei Variablen des Aufrufers in aufsteigende Reihenfolge bringt. Verwende dafür die Methode `Tausche` von oben. Vergleiche mit der Tupel-Variante `Sortiere` aus den Gültigkeitsbereichen: Welche würdest du lieber benutzen, und warum?
{: .notice--info}

Übung: Schreibe eine Methode `bool TryLiesNote(string eingabe, out double note)` nach dem `TryParse`-Muster. Sie soll nur dann `true` liefern, wenn die Eingabe eine Zahl ist *und* einer gültigen Note entspricht (1.0, 1.3, 1.7, 2.0, …, 4.0 oder 5.0). Nutze sie in einer Schleife, die so lange Noten einliest, bis der Benutzer eine leere Zeile eingibt, und gib am Ende den Durchschnitt aus.
{: .notice--info}

## Weitere Quellen

- [Parameterübergabe – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/passing-parameters)
- [ref – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/ref)
- [out-Parametermodifizierer – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/out-parameter-modifier)
