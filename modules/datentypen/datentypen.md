---
title: "Primitive Datentypen in C#"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bevor ein Programm irgendetwas sinnvoll tun kann, muss es Daten speichern – und dabei spielt es eine große Rolle, ob es sich um eine ganze Zahl, eine Kommazahl oder einen Text handelt. C# ist eine streng typisierte Sprache: jede Variable bekommt von Anfang an einen festen Typ, der zur Laufzeit nicht mehr wechselt.

## Überblick

In C# hat jede Variable einen festen Datentyp. Die wichtigsten primitiven Typen:

| Typ | Beschreibung | Beispiel |
| :--- | :--- | :--- |
| `int` | Ganze Zahl (32-bit) | `42`, `-7` |
| `long` | Ganze Zahl (64-bit) | `1_000_000_000L` |
| `double` | Gleitkommazahl (64-bit) | `3.14`, `-0.5` |
| `float` | Gleitkommazahl (32-bit) | `3.14f` |
| `bool` | Wahrheitswert | `true`, `false` |
| `char` | Einzelnes Zeichen (UTF-16) | `'A'`, `'\n'` |
| `string` | Zeichenkette | `"Hallo"` |

## Deklaration und Initialisierung

```csharp
int alter = 21;
double temperatur = 36.6;
bool istStudent = true;
char ersterBuchstabe = 'H';
string name = "HTW Berlin";
```

## `var` – implizite Typisierung

```csharp
var zahl = 42;       // C# erkennt: int
var text = "Hallo";  // C# erkennt: string
```

`var` ist kein dynamischer Typ – der Typ wird zur Kompilierzeit festgelegt.
{: .notice--primary}

Übung: Deklariere Variablen für Name, Alter, Matrikelnummer und GPA eines Studierenden. Welche Typen wählst du?
{: .notice--info}
