---
title: "Ternärer Operator"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal ist eine Entscheidung so simpel, dass ein vollständiger `if-else`-Block übertrieben wirkt – zum Beispiel wenn man nur zwischen zwei Texten oder zwei Zahlen wählen muss. Der ternäre Operator fasst genau das in eine einzige, kompakte Zeile.

## Syntax

```csharp
Bedingung ? WertWennTrue : WertWennFalse
```

## Vergleich mit if-else

```csharp
// Mit if-else:
string status;
if (punkte >= 50)
    status = "bestanden";
else
    status = "nicht bestanden";

// Mit ternärem Operator:
string status = punkte >= 50 ? "bestanden" : "nicht bestanden";
```

## Praktische Beispiele

```csharp
int alter = 20;
string kategorie = alter >= 18 ? "Erwachsen" : "Minderjährig";

int a = 7, b = 3;
int maximum = a > b ? a : b;

Console.WriteLine($"Max: {maximum}"); // 7
```

## Wann verwenden?

Der ternäre Operator eignet sich für **einfache Entweder-Oder-Zuweisungen**. Bei komplexerer Logik lieber `if-else` verwenden – Lesbarkeit geht vor.

```csharp
// Gut lesbar:
string vorzeichen = zahl >= 0 ? "+" : "-";

// Schlecht: verschachtelte ternäre Operatoren vermeiden
string note = p >= 90 ? "A" : p >= 75 ? "B" : p >= 60 ? "C" : "F"; // schwer lesbar
```
