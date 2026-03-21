---
title: "Typkonvertierung"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Im Alltag rechnen wir problemlos mit ganzen Zahlen und Dezimalzahlen zusammen – der Computer muss dabei aber immer genau wissen, in welchem Format ein Wert vorliegt. Typkonvertierung beschreibt, wie C# Werte von einem Typ in einen anderen umwandelt, und wann das automatisch passiert oder explizit angefordert werden muss.

## Implizite Konvertierung

Wenn eine Konvertierung garantiert verlustfrei ist, führt C# sie automatisch durch – ohne dass man etwas schreiben muss. Das funktioniert immer dann, wenn der Zieltyp den Wertebereich des Quelltyps vollständig abdeckt:

```csharp
int i = 42;
long l = i;       // int → long: passt immer rein
double d = i;     // int → double: kein Datenverlust
float f = i;      // int → float: funktioniert, aber Genauigkeit begrenzt
```

Die implizit erlaubten Konvertierungen in C# folgen einer festen Hierarchie (gestrichelte Pfeile: `decimal` hat keinen impliziten Weg nach `float`/`double`):

![Hierarchie impliziter Typkonvertierungen](/assets/images/typkonvertierung_implicit.svg)

Gemischte Ausdrücke werden automatisch auf den „größeren" Typ gebracht:

```csharp
int a = 3;
double b = 1.5;
double ergebnis = a + b;  // a wird implizit zu double – Ergebnis: 4.5
```

Ein häufiger Stolperstein: Division zweier `int`-Werte bleibt ganzzahlig, auch wenn das Ergebnis in einer `double`-Variable landet:

```csharp
double anteil = 1 / 4;         // 0.0  – ganzzahlige Division zuerst!
double anteil2 = 1.0 / 4;      // 0.25 – ein Operand als double reicht
double anteil3 = (double)1 / 4; // 0.25 – expliziter Cast erzwingt es
```

## Explizite Konvertierung (Cast)

Wenn Datenverlust möglich ist, verlangt C# einen expliziten Cast mit `(Typ)`. Das ist ein bewusstes Signal: „Ich weiß, dass hier Information verloren gehen kann."

```csharp
double pi = 3.14159;
int ganzzahl = (int)pi;      // Nachkommastellen werden abgeschnitten, nicht gerundet!
Console.WriteLine(ganzzahl); // 3

double gross = 1e18;
int überlauf = (int)gross;   // undefiniertes Ergebnis – Wertebereich überschritten
```

Beim Cast zwischen numerischen Typen wird nie gerundet – der Nachkommaanteil fällt einfach weg.
{: .notice--warning}

## Strings konvertieren: Parse, TryParse und Convert

Konsoleneingaben und Dateien liefern immer Strings – diese müssen in den richtigen Typ umgewandelt werden. Dafür gibt es drei Wege, die sich im Fehlerverhalten unterscheiden.

**`Parse`** konvertiert direkt, wirft aber eine Exception wenn der String kein gültiger Wert ist:

```csharp
int i      = int.Parse("100");
double d   = double.Parse("3.14");
bool b     = bool.Parse("true");
DateTime t = DateTime.Parse("2025-03-21");
```

**`TryParse`** ist die sichere Variante: bei ungültigem Input gibt sie `false` zurück, statt abzustürzen. Das `out`-Keyword schreibt das Ergebnis direkt in eine Variable:

```csharp
string eingabe = Console.ReadLine();

if (int.TryParse(eingabe, out int zahl))
    Console.WriteLine($"Doppelt: {zahl * 2}");
else
    Console.WriteLine("Keine gültige Zahl.");
```

Für Benutzereingaben immer `TryParse` bevorzugen – `Parse` ist nur sicher, wenn der Eingabewert garantiert gültig ist.
{: .notice--primary}

**`Convert`** funktioniert ähnlich wie `Parse`, kann aber auch mit `null` umgehen (liefert dann `0` statt Exception) und konvertiert zwischen mehr Typen:

```csharp
int a    = Convert.ToInt32("42");
int b    = Convert.ToInt32(3.99);   // 4 – rundet (anders als Cast!)
string s = Convert.ToString(true);  // "True"
int c    = Convert.ToInt32(null);   // 0 – kein Fehler
```

Der wichtigste Unterschied zu `(int)`: `Convert.ToInt32` **rundet**, ein Cast **schneidet ab**:

```csharp
double x = 3.9;
int perCast    = (int)x;              // 3
int perConvert = Convert.ToInt32(x);  // 4
```

Übung: Schreibe ein Programm, das zwei Zahlen von der Konsole einliest und ihren Quotienten als `double` ausgibt. Behandle ungültige Eingaben mit `TryParse` und Division durch null.
{: .notice--info}
