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

Der ternäre Operator (auch Bedingungsoperator genannt) hat drei Teile — daher der Name. Er wird „ternär" genannt, weil er der einzige Operator in C# mit drei Operanden ist (im Gegensatz zu binären Operatoren wie `+` mit zwei Operanden):

```csharp
Bedingung ? WertWennTrue : WertWennFalse
```

Die Bedingung wird ausgewertet. Ist sie `true`, liefert der Ausdruck den Wert vor dem `:`. Ist sie `false`, den Wert danach. Wichtig: Der ternäre Operator ist ein **Ausdruck**, kein Statement — er liefert einen Wert zurück, den man direkt in einer Zuweisung oder Ausgabe verwenden kann.

## Vergleich mit if-else

Vergleicht man den ternären Operator mit einem äquivalenten `if-else`, wird der Unterschied deutlich: Statt vier Zeilen reicht eine einzige — bei gleicher Bedeutung:

```csharp
// Mit if-else (4 Zeilen):
string status;
if (punkte >= 50)
    status = "bestanden";
else
    status = "nicht bestanden";

// Mit ternärem Operator (1 Zeile):
string status = punkte >= 50 ? "bestanden" : "nicht bestanden";
```

In beiden Fällen wird `status` je nach Bedingung zugewiesen. Die ternäre Variante macht auf einen Blick klar, dass es hier um eine einfache Entweder-Oder-Zuweisung geht.

## Praktische Beispiele

Der ternäre Operator eignet sich überall dort, wo man einem Wert abhängig von einer Bedingung einen von zwei Werten zuweisen möchte. Man kann ihn auch direkt in String-Interpolation oder als Argument eines Methodenaufrufs verwenden:

```csharp
int alter = 20;
string kategorie = alter >= 18 ? "Erwachsen" : "Minderjährig";

int a = 7, b = 3;
int maximum = a > b ? a : b;

Console.WriteLine($"Max: {maximum}"); // 7

// Direkt in der Ausgabe:
Console.WriteLine($"Du bist {(alter >= 18 ? "volljährig" : "minderjährig")}.");
```

Steht der ternäre Operator innerhalb einer String-Interpolation, muss er in zusätzliche runde Klammern gesetzt werden, damit der Compiler das `:` nicht als Formatangabe interpretiert.
{: .notice--primary}

## Wann verwenden?

Der ternäre Operator eignet sich für **einfache Entweder-Oder-Zuweisungen** — wenn die Bedingung kurz ist und beide Werte in eine Zeile passen. Bei komplexerer Logik ist ein `if-else` lesbarer und sollte bevorzugt werden:

```csharp
// Gut lesbar — einfache Wahl zwischen zwei Werten:
string vorzeichen = zahl >= 0 ? "+" : "-";

// Schlecht — verschachtelte ternäre Operatoren vermeiden:
string note = p >= 90 ? "A" : p >= 75 ? "B" : p >= 60 ? "C" : "F";
```

Die verschachtelte Variante ist technisch korrekt, aber schwer zu lesen. Hier wäre eine `if-else if`-Kette oder ein `switch` die bessere Wahl.

## Weitere Quellen

- [Bedingungsoperator `?:` – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/conditional-operator)
- [Auswahloperatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/conditional-operator#conditional-ref-expression)
