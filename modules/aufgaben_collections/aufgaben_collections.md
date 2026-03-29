---
title: "🧩 Aufgaben und Beispiele: Collections"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmieren lernt man nicht nur durch Codezeilen tippen — sondern auch durch **Nachdenken**. Die folgenden Aufgaben trainieren *Computational Thinking*: die Fähigkeit, Probleme so zu strukturieren, dass ein Computer sie lösen kann. Dazu gehören Abstraktion, Zerlegung, Mustererkennung und Algorithmenentwurf. Nimm dir für jede Aufgabe Zeit, bevor du die Lösung aufklappst.

## Aufgabe 1 — Algorithmenentwurf (schwierig)

Zwei Wörter sind **Anagramme**, wenn sie aus exakt denselben Buchstaben bestehen (z.B. „Listen" und „Silent", oder „Aster" und „Stern"). Groß-/Kleinschreibung soll ignoriert werden.

Entwirf einen Algorithmus, der prüft ob zwei Wörter Anagramme sind. Nutze ein `Dictionary<char, int>` um die Buchstaben zu zählen.

**Tipp:** Zähle die Buchstaben des ersten Worts hoch und die des zweiten Worts runter. Was muss am Ende gelten?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Ansatz 1 (aufwändig) — Zwei Dictionaries vergleichen:**

Man könnte für jedes Wort ein eigenes `Dictionary<char, int>` bauen und dann prüfen, ob beide identisch sind. Das funktioniert, erfordert aber einen aufwändigen Vergleich zweier Dictionaries.

**Ansatz 2 (elegant) — Ein Dictionary, hoch- und runterzählen:**

Die Idee: Wir verwenden **ein einziges** Dictionary. Für jeden Buchstaben im ersten Wort zählen wir den Wert **hoch** (+1), für jeden Buchstaben im zweiten Wort zählen wir **runter** (−1). Wenn die Wörter Anagramme sind, heben sich alle Zähler gegenseitig auf — am Ende müssen **alle Werte 0** sein.

```
Beispiel: "Listen" und "Silent" (klein: "listen" und "silent")

Nach "listen":  l=1, i=1, s=1, t=1, e=1, n=1
Nach "silent":  l=0, i=0, s=0, t=0, e=0, n=0  ✓ Alles 0!
```

**Implementierung in C#:**

```csharp
static bool SindAnagramme(string wort1, string wort2)
{
    // Schneller Vorab-Check: unterschiedliche Länge → kein Anagramm
    if (wort1.Length != wort2.Length)
    {
        return false;
    }

    wort1 = wort1.ToLower();
    wort2 = wort2.ToLower();

    Dictionary<char, int> zaehler = new Dictionary<char, int>();

    // Buchstaben des ersten Worts hochzählen
    foreach (char c in wort1)
    {
        if (zaehler.ContainsKey(c))
        {
            zaehler[c]++;
        }
        else
        {
            zaehler[c] = 1;
        }
    }

    // Buchstaben des zweiten Worts runterzählen
    foreach (char c in wort2)
    {
        if (zaehler.ContainsKey(c))
        {
            zaehler[c]--;
        }
        else
        {
            zaehler[c] = -1;
        }
    }

    // Prüfen: Alle Werte müssen 0 sein
    foreach (int wert in zaehler.Values)
    {
        if (wert != 0)
        {
            return false;
        }
    }

    return true;
}
```

**Testfälle:**

```csharp
Console.WriteLine(SindAnagramme("Listen", "Silent")); // True
Console.WriteLine(SindAnagramme("Aster", "Stern"));   // True
Console.WriteLine(SindAnagramme("Hallo", "Welt"));    // False
Console.WriteLine(SindAnagramme("ABC", "AB"));         // False
```

**Warum `Dictionary<char, int>`?**

Ein Dictionary eignet sich hier perfekt, weil wir für **jeden Buchstaben** (Key) einen **Zählerstand** (Value) speichern müssen. Wir wissen vorher nicht, welche Buchstaben vorkommen — das Dictionary wächst dynamisch. Der Zugriff über `ContainsKey` und den Indexer `zaehler[c]` ist dabei sehr effizient.

</details>
