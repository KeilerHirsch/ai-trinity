[English](../01-problem.md) · 🌐 **Deutsch**

[← Zurück zum README](README.md) · **Problem** · [These](02-thesis.md) · [Bau-Guide](03-build-guide.md) · [Windows-Realität](04-windows-reality.md) · [FAQ](05-faq.md)

---

# Das Problem: ein brillanter Amnesie-Patient

Das Modell ist nicht der Engpass. Das Modell ist exzellent. Der Engpass ist alles *drumherum* — und wenn du nur das Modell verbesserst, bleibt dir ein brillanter Amnesie-Patient, der gelegentlich selbstbewusst danebenliegt.

Drei Fehlerbilder treten auf, geordnet danach, wie viel Schmerz sie über die Zeit verursachen.

## 1. Verlustbehafteter Handoff — die Amnesie

Lange Konversationen werden **kompaktiert**: die Runtime fasst frühere Turns zusammen, um ins Context-Fenster zu passen. Compaction ist per Design **verlustbehaftet** — sie behält die Essenz und droppt Details. Ein „frischer" Session-Start genauso.

Der Schaden ist nicht abstrakt. Stell dir eine Session vor, in der der Assistent ein wirklich scharfes, gut belegtes Verständnis eines komplexen Themas aufbaut, das dir wichtig ist — deine Lage, dein Projekt, deine Randbedingungen. Er denkt mit vollem Kontext, verbindet Dinge, die du nicht ausgesprochen hast, und liegt *richtig*.

Dann endet die Session.

Die nächste weiß nichts davon. Nicht die Schlüsse, nicht die Belege, nicht die Nuancen. Du erklärst neu — oder schlimmer, du tust es nicht, und der Assistent arbeitet still mit weniger als gestern. Das Nützlichste, was das System je produziert hat, ist das Erste, was es vergisst. **Kontinuität ist das Produkt — und Kontinuität ist genau das, was ein zustandsloser Chat nicht liefern kann.**

Handoff-Notizen und „fass für nächstes Mal zusammen"-Tricks lösen das nicht. Sie sind verlustbehaftet auf verlustbehaftet: die Zusammenfassung einer Zusammenfassung. Genau das Detail, das die gestrige Session gut machte, droppt ein Handoff.

## 2. Keine Methode — roher Chat hat keine Disziplin

Ein Chatfenster ist jedes Mal ein leerer Prompt. Kein erzwungener Workflow, kein Review-Schritt, kein „hast du das wirklich verifiziert", keine wiederverwendbare Prozedur für das, was du ständig tust. Du bekommst, was das Modell an dem Tag improvisiert.

Für Einzelfragen ist das okay. Für echte, wiederholte Arbeit — Software bauen, Recherche, alles mit Konsequenzen — ist Improvisation keine Methode. Du brauchst Skills, Sub-Agenten, Gates und Hooks, die den *Prozess* verlässlich machen, nicht nur die einzelne Antwort.

## 3. Unzuverlässige Modelle vergiften alles Nachgelagerte

Nicht alle Modelle sind gleich ehrlich. Ein Modell mit hoher Halluzinationsrate gibt dir nicht nur einmal eine falsche Antwort — wenn du es in deine Notizen, dein Gedächtnis, deine Recherche schreiben lässt, **vergiftet es den Brunnen**. Jede spätere Session rechnet dann über korrumpierte Eingaben und potenziert den Fehler.

Das ist die Falle hinter naiven „Multi-Modell"-Setups: Arbeit durch ein Modell zu leiten, das selbstbewusst Fakten erfindet, heißt, dass die Kosten seiner Fehler später auftauchen — woanders, schwerer nachzuvollziehen. Die Fehlerquote ist kein Detail. Sie ist das Fundament, auf dem alles andere sitzt.

## Warum eins zu fixen nicht reicht

- Gutes Modell ohne Gedächtnis → brillanter Amnesie-Patient (Fehler 1).
- Gutes Modell mit Gedächtnis, aber ohne Methode → schnelles, dauerhaftes Chaos (Fehler 2).
- Gedächtnis + Methode, aber ein verrauschtes Modell → ein gut organisiertes, dauerhaftes Archiv selbstbewussten Unsinns (Fehler 3).

Die Fehler greifen ineinander. Deshalb ist die Lösung kein einzelnes Tool, sondern **drei Säulen, die alle gleichzeitig halten müssen** — Thema der [These](02-thesis.md).
