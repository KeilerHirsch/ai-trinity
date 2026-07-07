[English](../02-thesis.md) · 🌐 **Deutsch**

[← Zurück zum README](README.md) · [Problem](01-problem.md) · **These** · [Bau-Guide](03-build-guide.md) · [Windows-Realität](04-windows-reality.md) · [FAQ](05-faq.md) · [In der Praxis](06-in-the-wild.md)

---

# Die These: drei Säulen, alle Pflicht

Ein verlässliches KI-Arbeitssetup braucht drei Dinge, die **gleichzeitig** wahr sind. Jede Säule fällt anders aus, wenn sie fehlt, und keine Säule kompensiert eine andere. Das ist die ganze Behauptung.

```
        Modell-Güte            (ist die Antwort vertrauenswürdig?)
            /\
           /  \
          /    \
  Fundament —— persistentes Gehirn
  (gibt es eine   (überlebt es die
   Methode?)       nächste Session?)
```

## Säule 1 — Ein nachweislich nicht dummes Modell

**Behauptung:** Die Fehlerquote ist die entscheidende Eigenschaft eines funktionierenden Modells — mehr als rohe Fähigkeit oder Papier-Benchmarks.

Ein Modell, um das du ein System baust, muss daran gemessen werden, wie oft es *falsch* liegt — besonders wie oft es selbstbewusst falsch liegt bei Dingen, die du nicht leicht prüfen kannst. Erfindet das Modell Fakten, erbt alles Nachgelagerte (dein Gedächtnis, deine Recherche, deine Entscheidungen) diesen Fehler und potenziert ihn.

Praktisch heißt das: Halluzinations-/Fehlerquote objektiv messen und konsequente Arbeit nicht durch Modelle leiten, die den Screen nicht bestehen — egal wie beeindruckend sie in einer Demo wirken.

→ Referenz: [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark) — ein objektiver Screen für Modell-Unsinn.

## Säule 2 — Ein diszipliniertes Fundament (Harness)

**Behauptung:** Ein Chatbot wird erst zum Werkzeug, wenn es erzwungene Methode drumherum gibt.

Roher Chat improvisiert. Ein Harness gibt dir wiederverwendbare Skills, spezialisierte Sub-Agenten, Review-Gates und Hooks, die automatisch feuern — damit der *Prozess* verlässlich ist, nicht nur die gelegentlich gute Antwort. Es ist der Unterschied zwischen „fragen und hoffen" und „eine Prozedur, die jedes Mal gleich läuft, mit eingebauter Verifikation".

→ Referenz: [ECC](https://github.com/affaan-m/ECC) — Skills, Agenten, Hooks, Slash-Commands und Review-Gates auf der Agent-Runtime.

## Säule 3 — Ein persistentes Gehirn

**Behauptung:** Da Handoff/Compaction architektonisch verlustbehaftet ist ([siehe Problem](01-problem.md)), ist die einzige Lösung ein Gedächtnis, das *außerhalb* der Konversation lebt und in jede Session zurückgeladen wird.

Ein persistenter Speicher — automatisch beschrieben während du arbeitest, und zu Beginn jeder Session zurückgelesen — macht aus dem Amnesie-Patienten etwas Kontinuierliches. Das mühsam erarbeitete Verständnis von gestern ist heute da, ohne dass du es neu erklärst. Der Speicher muss **kompromisslos auf Qualität achten** (siehe Säule 1): ein Gedächtnis, das Rauschen aufnimmt, ist schlimmer als kein Gedächtnis, weil es schlechte Daten zu „erinnerter Tatsache" wäscht.

→ Referenz: [MemPalace](https://github.com/MemPalace/mempalace) — ein lokales Vektor-Speicher-Gedächtnis, automatisch via Runtime-Hooks gesichert und beim Session-Start zurückgeladen.

## Warum alle drei, und in dieser Reihenfolge

| Du hast | Dir fehlt | Ergebnis |
|---------|-----------|----------|
| Modell | Methode + Gedächtnis | Brillanter Amnesie-Patient, der täglich improvisiert |
| Modell + Methode | Gedächtnis | Schnell, wiederholbar — aber startet jede Session bei null |
| Modell + Gedächtnis | Methode | Persistent, aber undiszipliniert und nicht verifizierbar |
| Methode + Gedächtnis | Vertrauenswürdiges Modell | Ein gut organisiertes, dauerhaftes Archiv selbstbewussten Unsinns |
| **Alle drei** | — | Ein System, das verlässlich und kontinuierlich ist — und mit der Zeit *besser* wird |

Die Säulen sind kein Menü. Sie sind ein Satz gleichzeitiger Bedingungen. Der Payoff — das, was es sich anfühlen lässt „als hätte es nie eine Pause gegeben" — erscheint nur, wenn alle drei halten.

Weiter: **[wie man es baut](03-build-guide.md)**.
