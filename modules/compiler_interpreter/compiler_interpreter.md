---
title: "Interpreter- vs. Compilersprachen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmiersprachen unterscheiden sich grundlegend darin, wie Quellcode in ausführbare Anweisungen umgewandelt wird.

## Interpretersprachen

Bei Interpretersprachen liest ein **Interpreter** den Quellcode zur Laufzeit und führt ihn direkt aus – Zeile für Zeile, ohne vorherige Übersetzung.

**Beispiel: Python-Programm starten**

```python
# hello.py
print("Hallo, Welt!")
```

```bash
$ python3 hello.py
Hallo, Welt!
```

Der Python-Interpreter (`python3`) liest `hello.py` und führt es sofort aus. Es entsteht keine separate ausführbare Datei.

Weitere Beispiele: JavaScript (Node.js), Ruby, PHP, Bash.

## Compilersprachen

Bei Compilersprachen übersetzt ein **Compiler** den gesamten Quellcode vorab in Maschinencode. Das Ergebnis ist eine ausführbare Datei, die direkt auf der Hardware läuft – ohne den ursprünglichen Quellcode.

**Beispiel: C-Programm kompilieren und ausführen**

```c
// hello.c
#include <stdio.h>
int main() {
    printf("Hallo, Welt!\n");
    return 0;
}
```

```bash
$ gcc hello.c -o hello   # Compiler erzeugt ausführbare Datei
$ ./hello                # Direktes Ausführen – kein Interpreter nötig
Hallo, Welt!
```

Weitere Beispiele: C++, Rust, Go (erzeugen ebenfalls native Binaries).

## C# und .NET: Ein Mittelweg

C# verfolgt einen zweistufigen Ansatz:

**Stufe 1 – Kompilierung durch den C#-Compiler:**
Der Quellcode wird nicht in Maschinencode übersetzt, sondern in eine plattformunabhängige Zwischensprache: die **CIL** (Common Intermediate Language), gespeichert in einer `.dll`- oder `.exe`-Datei.

```bash
$ dotnet build          # C#-Compiler erzeugt CIL (.dll)
$ dotnet run            # .NET Runtime führt die CIL aus
Hallo, Welt!
```

**Stufe 2 – JIT-Kompilierung zur Laufzeit:**
Die **.NET Runtime** (CLR – Common Language Runtime) übersetzt die CIL beim Start per **JIT-Compiler** (Just-In-Time) in nativen Maschinencode für die aktuelle Plattform.

```
Quellcode (.cs)
      │
      ▼  dotnet build / csc
   CIL (.dll)
      │
      ▼  JIT-Compiler (CLR zur Laufzeit)
Maschinencode
      │
      ▼
  Ausführung
```

## Vergleich auf einen Blick

| Eigenschaft | Interpreter (Python) | Compiler (C) | C# / .NET |
| :--- | :--- | :--- | :--- |
| Übersetzung | Keine vorab | Vollständig vorab | Zweistufig (CIL + JIT) |
| Portabilität | Hoch (Interpreter nötig) | Gering (plattformspezifisch) | Hoch (Runtime nötig) |
| Startgeschwindigkeit | Schnell | Sehr schnell | Schnell (JIT) |
| Ausführungsgeschwindigkeit | Langsamer | Sehr schnell | Sehr schnell |
| Fehler erkennbar | Erst zur Laufzeit | Bereits beim Kompilieren | Bereits beim Kompilieren |

Der letzte Punkt ist für Einsteiger besonders relevant: C# meldet viele Fehler **bereits beim Kompilieren**, bevor das Programm überhaupt gestartet wird.
{: .notice--primary}
