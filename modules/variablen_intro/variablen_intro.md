---
title: "Was ist eine Variable?"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programme müssen sich Dinge merken: den aktuellen Kontostand, den eingegebenen Namen, das Zwischenergebnis einer Berechnung. Dafür gibt es Variablen — benannte Speicherplätze im Arbeitsspeicher, die einen Wert aufnehmen und ihn bei Bedarf wieder herausgeben.

## Deklaration und Zuweisung

Eine Variable wird mit Typ und Name deklariert. Der Wert wird per `=` zugewiesen:

```csharp
int alter;         // Deklaration: Speicherplatz für eine ganze Zahl anlegen
alter = 21;        // Zuweisung: Wert hineinspeichern

int punkte = 100;  // beides in einem Schritt
```

Der Typ legt fest, welche Art von Wert die Variable aufnehmen darf — eine ganze Zahl (`int`), eine Kommazahl (`double`), einen Text (`string`) usw.

## Wert ändern

Der Name „Variable" kommt daher, dass der gespeicherte Wert jederzeit geändert werden kann:

```csharp
int punkte = 0;
punkte = 10;
punkte = 25;
Console.WriteLine(punkte); // 25
```

Der alte Wert wird dabei überschrieben — er ist danach nicht mehr zugänglich.

## Mehrere Variablen kombinieren

Variablen verschiedener Typen können in einem Programm zusammenarbeiten:

```csharp
string name = "Anna";
int alter = 21;
double gpa = 1.7;

Console.WriteLine($"{name} ist {alter} Jahre alt und hat einen GPA von {gpa}.");
```

Übung: Deklariere Variablen für deinen Namen, dein Alter und deine Lieblingsfarbe. Gib alle drei in einem Satz aus.
{: .notice--info}
