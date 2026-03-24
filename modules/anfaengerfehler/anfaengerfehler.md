---
title: "Häufige Anfängerfehler in C#"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manche Fehler begegnen fast allen, die C# neu lernen — und das Tückische ist, dass viele davon keinen Compilerfehler auslösen, sondern still falsche Ergebnisse liefern. Wer diese Klassiker kennt, erkennt sie sofort, wenn er ihnen begegnet.

## 1. `=` statt `==`

Die Verwechslung von Zuweisung (`=`) und Vergleich (`==`) ist einer der häufigsten Fehler überhaupt. In C# erzeugt dies meist einen Compilerfehler, weil eine Zuweisung kein `bool` liefert — aber in bestimmten Kontexten kann der Fehler durchrutschen:

```csharp
int x = 5;
if (x = 10)  // Fehler! = ist Zuweisung, nicht Vergleich
{ ... }

if (x == 10) // Richtig: == ist Vergleich
{ ... }
```

## 2. Ganzzahldivision

Wenn man zwei `int`-Werte dividiert, ist das Ergebnis immer ein `int` — die Nachkommastellen werden abgeschnitten, nicht gerundet. Das passiert **bevor** das Ergebnis in die Variable gespeichert wird, weshalb auch eine `double`-Variable nichts daran ändert:

```csharp
int a = 7, b = 2;
double ergebnis = a / b;        // Ergibt 3.0, nicht 3.5!
double korrekt  = (double)a / b; // Ergibt 3.5
```

Die Lösung: mindestens einen der beiden Operanden zum `double` machen — entweder durch einen Cast oder indem man ein Literal mit Dezimalpunkt schreibt (`7.0 / 2`).

## 3. Operatorrangfolge vergessen

Punkt vor Strich gilt auch in C#. Wer Klammern vergisst, bekommt leise falsche Ergebnisse — der Compiler meldet keinen Fehler:

```csharp
double durchschnitt = 10 + 20 + 30 / 3; // Ergibt 40, nicht 20!
double korrekt = (10 + 20 + 30) / 3.0;  // Ergibt 20
```

Im ersten Fall wird nur `30 / 3` berechnet (= 10), dann `10 + 20 + 10 = 40`. Die Klammern im zweiten Fall erzwingen die gewünschte Reihenfolge.

## 4. String-Vergleich und Groß-/Kleinschreibung

Der `==`-Operator funktioniert für Strings in C#, unterscheidet aber zwischen Groß- und Kleinschreibung. Wenn ein Benutzer "Ja" statt "ja" eingibt, schlägt der Vergleich fehl:

```csharp
string eingabe = Console.ReadLine();
if (eingabe == "ja")   // Funktioniert — aber "Ja" oder "JA" wird nicht erkannt!
{ ... }

// Sicherer — Groß-/Kleinschreibung ignorieren:
if (eingabe.Equals("ja", StringComparison.OrdinalIgnoreCase))
{ ... }
```

## 5. Variable nicht initialisiert

C# erzwingt, dass lokale Variablen vor der ersten Verwendung einen Wert haben. Anders als in manchen anderen Sprachen gibt es keinen "Standardwert" für lokale Variablen — der Compiler verweigert die Übersetzung:

```csharp
int summe;
Console.WriteLine(summe); // Compilerfehler: nicht initialisiert
```

Die Lösung ist einfach: `int summe = 0;`. Es lohnt sich, Variablen immer direkt bei der Deklaration zu initialisieren.

## 6. Off-by-one bei Schleifen

Der "Off-by-one-Fehler" ist ein Klassiker: Die Schleife läuft einmal zu oft oder einmal zu wenig. Die Ursache liegt fast immer in der Verwechslung von `<` und `<=` oder einem falschen Startwert:

```csharp
// Soll 1 bis 10 ausgeben:
for (int i = 1; i <= 10; i++)  // Richtig: <= 10
for (int i = 1; i < 10; i++)   // Falsch: gibt nur 1 bis 9 aus
for (int i = 0; i <= 10; i++)  // Falsch: gibt 0 bis 10 (11 Zahlen) aus
```

Ein guter Trick zur Vermeidung: Sich bei jedem Schleifenkopf fragen "Was ist der erste und was der letzte Wert von `i`?" und das kurz an einem Beispiel durchrechnen.

## 7. Semikolon nach `if` oder `for`

Ein Semikolon direkt nach einem `if` oder `for` erzeugt eine leere Anweisung — der nachfolgende Block wird **immer** ausgeführt, unabhängig von der Bedingung. Der Compiler meldet keinen Fehler, weil die Syntax technisch gültig ist:

```csharp
if (x > 0);          // Semikolon beendet die if-Anweisung sofort!
{
    Console.WriteLine("positiv"); // Wird IMMER ausgeführt
}
```

Dasselbe passiert bei Schleifen: `for (int i = 0; i < 10; i++);` läuft die Schleife durch, ohne je den Rumpf auszuführen.
{: .notice--warning}

## Weitere Quellen

- [Häufige C#-Fehler und Lösungen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/misc/)
