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

Stell dir eine Variable wie eine Schublade vor: Der **Typ** bestimmt die Größe der Schublade, der **Name** steht auf dem Etikett, und der **Wert** ist das, was darin liegt. Nicht jeder Inhalt passt in jede Schublade — eine Kommazahl passt nicht in eine Schublade, die nur für ganze Zahlen ausgelegt ist.

## Deklaration und Zuweisung

Eine Variable wird mit Typ und Name deklariert. Der Wert wird per `=` zugewiesen:

```csharp
int alter;         // Schublade anlegen: Platz für eine ganze Zahl
alter = 21;        // Inhalt einlegen

int punkte = 100;  // beides in einem Schritt
```

Der Typ legt fest, welche Art von Wert die Variable aufnehmen darf — eine ganze Zahl (`int`), eine Kommazahl (`double`), einen Text (`string`) usw. Der Compiler prüft das bereits beim Übersetzen: Wer versucht, einen Text in eine `int`-Schublade zu legen, bekommt sofort einen Fehler.

## Wert ändern

Der Name „Variable" kommt daher, dass der gespeicherte Wert jederzeit geändert werden kann — die Schublade wird einfach neu befüllt:

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
double note = 1.7;

Console.WriteLine($"{name} ist {alter} Jahre alt und hat die Note {note}.");
```

Übung: Deklariere Variablen für deinen Namen, dein Alter und deine Lieblingsfarbe. Gib alle drei in einem Satz aus.
{: .notice--info}

## Weitere Quellen

- [Deklarationsanweisungen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/statements/declarations)
