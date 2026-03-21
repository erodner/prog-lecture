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

Manche Fehler begegnen fast allen, die C# neu lernen – und das Tückische ist, dass viele davon keinen Kompilierfehler auslösen, sondern still falsche Ergebnisse liefern. Wer diese Klassiker kennt, erkennt sie sofort, wenn er ihnen begegnet.

## 1. `=` statt `==`

```csharp
int x = 5;
if (x = 10)  // Fehler! = ist Zuweisung, nicht Vergleich
{ ... }

if (x == 10) // Richtig: == ist Vergleich
{ ... }
```

## 2. Ganzzahldivision

```csharp
int a = 7, b = 2;
double ergebnis = a / b;        // Ergibt 3.0, nicht 3.5!
double korrekt  = (double)a / b; // Ergibt 3.5
```

Bei Division zweier `int`-Werte wird das Ergebnis immer **abgeschnitten**, auch wenn es in eine `double`-Variable gespeichert wird.

## 3. Operatorrangfolge vergessen

```csharp
double durchschnitt = 10 + 20 + 30 / 3; // Ergibt 40, nicht 20!
double korrekt = (10 + 20 + 30) / 3.0;  // Ergibt 20
```

## 4. String-Vergleich mit `==`

```csharp
string eingabe = Console.ReadLine();
if (eingabe == "ja")   // Funktioniert in C# – aber Groß-/Kleinschreibung beachten!
{ ... }

// Sicherer – Groß-/Kleinschreibung ignorieren:
if (eingabe.Equals("ja", StringComparison.OrdinalIgnoreCase))
{ ... }
```

## 5. Variable nicht initialisiert

```csharp
int summe;
Console.WriteLine(summe); // Compilerfehler: nicht initialisiert
```

Lokale Variablen müssen vor der ersten Verwendung einen Wert haben.

## 6. Off-by-one bei Schleifen

```csharp
// Soll 1 bis 10 ausgeben:
for (int i = 1; i <= 10; i++)  // Richtig: <= 10
for (int i = 1; i < 10; i++)   // Falsch: gibt nur 1 bis 9 aus
for (int i = 0; i <= 10; i++)  // Falsch: gibt 0 bis 10 (11 Zahlen) aus
```

## 7. Semikolon nach `if`

```csharp
if (x > 0);          // Semikolon beendet die if-Anweisung sofort!
{
    Console.WriteLine("positiv"); // Wird IMMER ausgeführt
}
```

Dieser Fehler erzeugt keinen Compilerfehler – er ist ein Logikfehler.
{: .notice--warning}
