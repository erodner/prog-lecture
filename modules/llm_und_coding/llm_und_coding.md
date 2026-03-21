---
title: "KI-Assistenten und Programmieren – Brauche ich das noch?"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Große Sprachmodelle so wie sie in GitHub Copilot, ChatGPT oder Claude integriert sind, können auf Anfrage Code erzeugen. Das wirft eine naheliegende Frage auf: **Warum sollte ich Programmieren überhaupt noch selbst lernen?**

## Was LLMs tatsächlich können

LLMs sind beeindruckende Werkzeuge. Sie können:

- Boilerplate-Code und Standardmuster generieren
- bekannte Algorithmen in einer gewünschten Sprache aufschreiben
- Fehlermeldungen erklären und Lösungsvorschläge machen
- Dokumentation und Tests für vorhandenen Code schreiben

Das klingt nach viel. Aber es gibt entscheidende Einschränkungen.

## Was LLMs nicht können

LLMs erzeugen Code, indem sie statistische Muster aus Milliarden von Textbeispielen kombinieren. Sie **verstehen** das Problem nicht (jedenfalls nicht so wie wir es tun) – sie produzieren etwas, das wie eine Lösung *aussieht*.

Das hat praktische Konsequenzen:

- **Falsche Antworten wirken überzeugend.** LLMs „halluzinieren" – sie erfinden Funktionen, die nicht existieren, oder schreiben Code, der subtile Logikfehler enthalten kann - meist wie ein Praktikant der sehr von sich überzeugt ist.
- **Du kannst Fehler nur erkennen, wenn du die Grundlagen beherrschst.** Wer nicht verstehen kann, was der generierte Code tut und wie er funktioniert, kann auch nicht beurteilen, ob er korrekt ist.
- **Neue und domänenspezifische Probleme sind schwierig.** Bei ungewöhnlichen Anforderungen, sicherheitskritischen Systemen oder firmeneigenem Kontext liefern LLMs aktuell noch deutlich schlechtere Ergebnisse.
- **Debugging bleibt deine Aufgabe.** Ein LLM kann bei einem Fehler raten und unterstützen – du musst wissen, wo und warum er auftritt.

Ein LLM ist ein Werkzeug – so wie ein Taschenrechner. Wer nicht rechnen kann, merkt nicht, wenn das Ergebnis falsch ist.
{: .notice--warning}

## Das Kompetenzproblem

Stell dir vor, du bittest ein LLM, eine Funktion zu schreiben, die eine Liste von Studierenden nach Notendurchschnitt sortiert. Es liefert etwas. Aber:

- Ist der Algorithmus effizient genug für große Datensätze?
- Werden Randfälle korrekt behandelt (leere Liste, gleiche Noten)?
- Ist der Code lesbar und wartbar?
- Passt er zur restlichen Architektur eurer Anwendung?

Alle diese Fragen erfordern **fundamentale Programmierkenntnisse**. Ohne sie kannst du die Ausgabe des LLM weder bewerten noch sinnvoll einsetzen und in größere komplexe Systeme integrieren.

## Computational Thinking – die eigentliche Kompetenz

Was Programmieren langfristig wertvoll macht, ist nicht das Tippen von Syntax. Es ist die Fähigkeit, Probleme so zu durchdenken und zu strukturieren, dass sie lösbar werden. Diese Denkweise nennt sich **Computational Thinking** und besteht aus vier Kernprinzipien:

**Zerlegung (Decomposition)**
Ein komplexes Problem in kleinere, handhabbare Teilprobleme aufteilen.
*Beispiel: „Schreibe ein Spiel" → Eingabe verarbeiten, Zustand aktualisieren, Ausgabe erzeugen.*

**Mustererkennung (Pattern Recognition)**
Gemeinsamkeiten zwischen Problemen erkennen und vorhandene Lösungen wiederverwenden.
*Beispiel: Das Durchsuchen einer Liste ist immer ähnlich – egal ob nach Namen, Noten oder Preisen gesucht wird.*

**Abstraktion (Abstraction)**
Unwichtige Details weglassen und sich auf das Wesentliche konzentrieren.
*Beispiel: Eine Methode `BerechneNotendurchschnitt(noten)` – der Aufrufer muss nicht wissen, wie sie intern funktioniert.*

**Algorithmenentwicklung (Algorithm Design)**
Eine präzise, schrittweise Handlungsanweisung entwickeln, die das Problem löst.
*Beispiel: Wie sortiere ich eine Liste? Welche Schritte müssen in welcher Reihenfolge erfolgen?*

Diese Fähigkeiten lassen sich nicht vollständig delegieren – weder an ein LLM noch an eine andere Person. Sie sind der Kern dessen, was einen Softwareentwickler oder eine Softwareentwicklerin ausmacht.
{: .notice--primary}

## Die richtige Einstellung

LLMs werden ein Teil eurer Arbeitswelt sein – das ist keine Frage. Aber der Wert, den ihr als Entwicklerinnen und Entwickler einbringt, liegt darin, dass ihr:

1. **Probleme präzise formulieren** könnt (schlechte Prompts → schlechter Code)
2. **Ergebnisse kritisch bewerten** könnt
3. **Eigenständig debuggen und verbessern** könnt
4. **Verantwortung für die Korrektheit** des Codes übernehmt

Wer die Grundlagen nicht beherrscht, ist vollständig abhängig von einem Werkzeug, das keine Garantien gibt. Wer sie beherrscht, kann LLMs sinnvoll einsetzen und dabei deutlich produktiver sein.

**Ziel dieser Vorlesung ist nicht, euch das Tippen beizubringen – sondern das Denken.**

## Wie funktionieren LLMs eigentlich?

Wer verstehen möchte, was unter der Haube eines LLM passiert, dem empfehle ich die folgende visuelle Einführung von 3Blue1Brown:

- [But what is a GPT? Visual intro to transformers](https://www.youtube.com/watch?v=LPZh9BOjkQs) – kurze Einführung in GPT und Transformer
- [Transformers, the tech behind LLMs](https://www.youtube.com/watch?v=wjZofJX0v4M) – Tokenisierung, Embeddings und die Transformer-Architektur (ca. 27 Min.)
- [Attention in transformers, step-by-step](https://www.youtube.com/watch?v=eMlx5fFNoYc) – der Attention-Mechanismus im Detail (ca. 27 Min.)

Das Verständnis dieser Videos ist keine Voraussetzung für die Vorlesung – aber es hilft, LLMs als das einzuordnen, was sie sind: sehr leistungsfähige statistische Mustererkennungssysteme, keine denkenden Maschinen.

[![XKCD 1838 – Machine Learning](https://imgs.xkcd.com/comics/machine_learning.png)](https://xkcd.com/1838/)

[„Machine Learning"](https://xkcd.com/1838/) – Randall Munroe, [xkcd.com](https://xkcd.com), lizenziert unter [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/)
