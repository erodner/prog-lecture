---
title: "Arithmetische Operatoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Addieren, subtrahieren, den Rest einer Division bestimmen – arithmetische Operatoren erledigen in C# all das, was man vom Taschenrechner kennt. Allerdings gibt es ein paar Besonderheiten, die beim Rechnen mit ganzen Zahlen schnell zu unerwarteten Ergebnissen führen können.

## Grundrechenarten

Die fünf arithmetischen Grundoperatoren in C# sind `+`, `-`, `*`, `/` und `%`. Die ersten drei verhalten sich genau wie in der Mathematik — bei Division und Modulo lohnt sich ein genauerer Blick:

```csharp
int a = 17, b = 5;

Console.WriteLine(a + b);  // 22  – Addition
Console.WriteLine(a - b);  // 12  – Subtraktion
Console.WriteLine(a * b);  // 85  – Multiplikation
Console.WriteLine(a / b);  // 3   – Ganzzahldivision (Nachkommastellen abgeschnitten!)
Console.WriteLine(a % b);  // 2   – Modulo (Rest der Division)
```

Das Ergebnis von `17 / 5` ist `3` und nicht `3.4` — weil beide Operanden `int` sind, liefert C# auch ein `int`-Ergebnis. Die Nachkommastellen werden dabei nicht gerundet, sondern einfach abgeschnitten. Das ist eine der häufigsten Fehlerquellen für Anfänger.

## Ganzzahldivision vs. Gleitkommadivision

Ob C# eine Ganzzahl- oder Gleitkommadivision durchführt, hängt allein vom Typ der Operanden ab — nicht davon, in welcher Variable das Ergebnis landet:

```csharp
int x = 7, y = 2;
Console.WriteLine(x / y);           // 3   (int / int = int)
Console.WriteLine((double)x / y);   // 3.5 (cast auf double zuerst)
Console.WriteLine(7.0 / 2);         // 3.5 (ein Operand ist double)
```

Es reicht, **einen** der beiden Operanden zum `double` zu machen — der andere wird dann automatisch mitkonvertiert. Das kann durch einen expliziten Cast `(double)x` geschehen oder indem man ein Literal mit Dezimalpunkt schreibt (`7.0` statt `7`).

Wenn das Ergebnis einer Division eine Kommazahl sein soll, muss mindestens ein Operand ein Gleitkommatyp sein. `int / int` liefert **immer** ein `int`.
{: .notice--warning}

## Modulo – praktische Anwendungen

Der Modulo-Operator `%` liefert den **Rest** einer ganzzahligen Division. Mathematisch: wenn `a / b = q` Rest `r`, dann ist `a % b = r`. Man liest `a % b` als „a modulo b".

Der einfachste Anwendungsfall: die letzte Ziffer einer Zahl extrahieren:

```csharp
int n = 1234;
Console.WriteLine(n % 10);  // 4 – letzte Ziffer
```

Ein etwas umfangreicheres Beispiel — Sekunden in Stunden, Minuten und Restsekunden umrechnen. Hier arbeiten Ganzzahldivision und Modulo Hand in Hand:

```csharp
int sekunden = 3725;
int stunden  = sekunden / 3600;         // 1 (ganze Stunden)
int minuten  = (sekunden % 3600) / 60;  // 2 (verbleibende ganze Minuten)
int rest     = sekunden % 60;           // 5 (verbleibende Sekunden)
Console.WriteLine($"{stunden}h {minuten}min {rest}s"); // 1h 2min 5s
```

Das Zusammenspiel ist: `/` liefert den ganzzahligen Anteil, `%` den Rest. Gemeinsam zerlegen sie einen Wert in größere und kleinere Einheiten.

## Inkrement und Dekrement

Eine Variable um genau 1 zu erhöhen oder zu verringern ist so häufig, dass C# eigene Operatoren dafür hat: `++` (Inkrement) und `--` (Dekrement).

```csharp
int i = 5;
i++;  // i = 6  (Postfix-Inkrement: erst den Wert nutzen, dann erhöhen)
i--;  // i = 5  (Postfix-Dekrement)
++i;  // i = 6  (Präfix-Inkrement: erst erhöhen, dann den Wert nutzen)
```

In den meisten Fällen — vor allem in Schleifen — ist der Unterschied zwischen `i++` und `++i` irrelevant. Er wird erst wichtig, wenn der Ausdruck gleichzeitig in einer Zuweisung steht, etwa `int b = a++;`. Diese Art der Kombination aus Zuweisung und Inkrements ist aber generell aus Gründen der Übersichtlichkeit nicht zu empfehlen! Für den Anfang reicht es, immer `i++` zu verwenden.

Übung: Schreibe ein Programm, das eine Anzahl von Sekunden von der Konsole einliest und in Stunden, Minuten und Sekunden umrechnet. Teste es mit den Werten 0, 59, 3600 und 86400 (ein ganzer Tag).
{: .notice--info}

## Weitere Quellen

- [Arithmetische Operatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/arithmetic-operators)
