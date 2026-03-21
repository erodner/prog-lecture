---
title: "Variablen und Konstanten"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Variablen sind die Grundbausteine jedes Programms – sie geben Daten einen Namen und machen Code lesbar. Manchmal soll ein Wert sich nie ändern, etwa die Mehrwertsteuer oder die maximale Anzahl von Versuchen: dafür gibt es Konstanten, die den Code gleichzeitig sicherer und selbsterklärender machen.

## Variablen

Eine Variable wird mit Typ, Name und optionalem Anfangswert deklariert:

```csharp
int anzahl = 5;
double preis = 9.99;
bool istAktiv = true;
string titel = "Programmierung 1";
```

## `var` – implizite Typisierung

Mit `var` erkennt der Compiler den Typ automatisch aus dem zugewiesenen Wert:

```csharp
var anzahl = 5;          // int
var preis = 9.99;        // double
var titel = "Prog 1";   // string
```

`var` ist praktisch, aber der Typ ist unveränderlich – er wird beim Kompilieren festgelegt, nicht zur Laufzeit.
{: .notice--primary}

## Konstanten

Eine Konstante wird mit `const` deklariert und kann nach der Initialisierung nicht mehr verändert werden:

```csharp
const double Pi = 3.14159;
const int MaxVersuche = 3;

Pi = 3.0; // Compilerfehler! Konstanten sind unveränderlich.
```

Konstanten eignen sich für Werte, die sich im Programm nie ändern sollen – z.B. Grenzwerte, Konfigurationswerte oder mathematische Konstanten.

## Namenskonventionen

| Art | Konvention | Beispiel |
| :--- | :--- | :--- |
| Lokale Variable | camelCase | `anzahlStudierende` |
| Konstante | PascalCase | `MaxPunkte` |
| Parameter | camelCase | `vorname` |

Übung: Was ist der Unterschied zwischen `var x = 5;` und `const int x = 5;`? Was passiert, wenn du versuchst, beide danach zu verändern?
{: .notice--info}
