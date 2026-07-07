[English](../03-build-guide.md) · 🌐 **Deutsch**

[← Zurück zum README](README.md) · [Problem](01-problem.md) · [These](02-thesis.md) · **Bau-Guide** · [Windows-Realität](04-windows-reality.md) · [FAQ](05-faq.md) · [In der Praxis](06-in-the-wild.md)

---

# Der Bau-Guide

Das ist der reproduzierbare Pfad. Er gibt dir **keine** Config zum Reinkopieren — dein Setup soll deins sein. Er sagt dir, was du zusammenbaust, in welcher Reihenfolge, und wie du jede Säule wirklich verifizierst.

> Die Reihenfolge zählt. Erst Säule 1 (damit du kein Rauschen abspeicherst), dann Säule 2 (damit du Methode hast), dann Säule 3 (damit es persistiert). Jede verifizieren, bevor du weitergehst.

## Voraussetzungen

- Eine Agent-Runtime, die du täglich nutzt (z.B. Claude Code im Editor oder Terminal).
- Ein leistungsfähiges Workhorse-Modell. Nimm das stärkste verfügbare und halte es stabil.
- Sicherheit mit Shell, `git` und dem Editieren von JSON-Config. Das ist die Grundlinie.

## Säule 1 — Das Modell screenen (nicht überspringen)

**Ziel:** die Fehlerquote deines Modells kennen, bevor du ihm Gedächtnis anvertraust.

1. Lies den Screen: [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark). Versteh, *was* er misst (selbstbewusstes Danebenliegen), nicht nur den Score.
2. Entscheide dein Workhorse-Modell und schreib auf, *warum* es deine Hürde nimmt.
3. Mach eine Regel daraus: konsequente oder ins-Gedächtnis-schreibende Arbeit nie durch ein Modell leiten, das den Screen nicht besteht. Lass „Multi-Modell"-Setups fallen, in denen ein schwaches Modell mitstimmt — seine Fehler kosten dich später (siehe [Problem §3](01-problem.md)).

**Verifizieren:** Du kannst in einem Satz sagen, welchem Modell du Schreib-ins-Gedächtnis-Arbeit anvertraust und warum.

## Säule 2 — Das Fundament installieren (Harness)

**Ziel:** Improvisation durch erzwungene Methode ersetzen.

1. Installiere [ECC](https://github.com/affaan-m/ECC) als Plugin/Marketplace für deine Runtime (folge dem README für deine Plattform).
2. Bestätige, dass die Schicht lebt: seine Skills, Agenten und Slash-Commands erscheinen in deiner Session und laufen wirklich.
3. Übernimm die Teile, die zu *deiner* Arbeit passen — Review-Gates, Build/Test-Skills, Security-Guards — und schalte aus, was du nicht nutzt. Ein schlankes, bewusstes Set schlägt den vollen Feuerwehrschlauch.
4. Füge Guardrail-Hooks für die gefährlichen Defaults hinzu: Secret-Datei-Lesen blocken, destruktive Befehle verweigern, alle Hook-Pfade workspace-relativ halten, damit ein verschobener Ordner sie nicht bricht.

**Verifizieren:** ein Slash-Command läuft Ende-zu-Ende durch; ein Guard-Hook blockt tatsächlich etwas, das er blocken soll.

## Säule 3 — Das persistente Gehirn aufstellen

**Ziel:** Gedächtnis, das sich beim Arbeiten selbst schreibt und beim Session-Start zurücklädt.

1. Installiere [MemPalace](https://github.com/MemPalace/mempalace) (oder einen gleichwertigen lokalen Memory-Store) und verbinde es als MCP-Server mit deiner Runtime.
2. **Auto-Save über Hooks verdrahten** — genau das überspringen Leute und wundern sich dann, warum das Gedächtnis leer ist:
   - ein **Stop**-Hook und ein **Pre-Compaction**-Hook, die die Notizen/den Zustand der Session schreiben, *bevor* Kontext verloren geht;
   - ein **Session-Start**-Hook, der den geretteten Kontext zurück injiziert.
3. **Sauber halten** (Säule 1 gilt für deinen eigenen Store): Secrets/Müll vor dem Ingest filtern, deduplizieren, damit wiederholte Ingests nicht aufblähen, und nie ein verrauschtes Modell ungeprüfte „Fakten" hineinschreiben lassen.
4. Führe einen winzigen, menschenlesbaren Index dessen, was gespeichert ist — damit du und der Assistent die Form des Gedächtnisses auf einen Blick seht.

**Verifizieren:** Beende eine Session, starte eine neue, und frag nach etwas, das nur die vorige wusste. Wenn es aus dem Gedächtnis antwortet, ohne dass du neu erklärst — das Gehirn hält.

## Alles zusammen

Ein Arbeitstag sieht jetzt so aus: Session startet → Gedächtnis + Instincts laden automatisch → du arbeitest mit Methode (Harness) → ein vertrauenswürdiges Modell denkt über korrekten Kontext → beim Beenden wird die Session gesichert, ohne dass du fragst. Morgen macht da weiter, wo heute aufhörte.

Diese Kontinuität ist der ganze Sinn. Wenn es funktioniert, fühlt es sich an, **als hätte es nie eine Pause gegeben**.

→ Bevor du auf Windows feierst, lies [Windows-Realität](04-windows-reality.md): ein Teil der Auto-Save-/Lern-Maschinerie hat echte Plattform-Lücken, mit Workarounds.
