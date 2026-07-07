[English](../06-in-the-wild.md) · 🌐 **Deutsch**

[← Zurück zum README](README.md) · [Problem](01-problem.md) · [These](02-thesis.md) · [Bau-Guide](03-build-guide.md) · [Windows-Realität](04-windows-reality.md) · [FAQ](05-faq.md) · **In der Praxis**

---

# In der Praxis: Tools, gebaut auf dieser Grundlage

Die drei Säulen sind nicht nur Theorie — sie erzeugen echte, konkrete Engineering-Probleme, sobald du wirklich versuchst, darin zu leben. Diese Seite verfolgt konkrete Tools, die direkt aus dem Befolgen dieser These entstanden sind, angefangen mit dem des Autors selbst.

## Schrödinger Sync — schließt eine echte Lücke in Säule 3

**Problem:** Säule 3 (das persistente Gehirn) hilft nur mit dem, was es tatsächlich erreicht. Claude Codes lokale Session-Transkripte landen als JSONL auf der Platte — trivial zu ingesten. Claude Desktop und Claudes Web-App nicht: diese Konversationen leben serverseitig, ohne lokale Datei, die ein Hook beobachten könnte. Jede Desktop-/Web-Session war konstruktionsbedingt unsichtbar für den Memory-Store — eine zweite Amnesie, versteckt hinter der ersten, die dieses Repo bereits löst.

**Was es macht:** [Schrödinger Sync](https://github.com/KeilerHirsch/schroedinger-sync) exportiert deine eigenen claude.ai-Konversationen, Projekt-Knowledge-Docs und den Inhalt der Memory-Funktion nach lokalem Markdown — schließt genau diese Lücke, sodass Desktop-/Web-Historie in dasselbe persistente Gehirn fließt, in das Claude Code bereits schreibt.

**Wie, kurz:** claude.ai sitzt hinter einer von Cloudflare verwalteten JS-Challenge, die headless-Automation nicht lösen kann. Das Tool entschlüsselt deinen eigenen Session-Cookie aus Claude Desktops lokalem, DPAPI-geschütztem Speicher, steuert dann ein echtes, *sichtbares* Chrome-Fenster über das Chrome DevTools Protocol — Chrome löst die Challenge selbst, das Tool macht dann same-origin API-Calls von innerhalb dieser Seite aus. Kein Credential berührt jemals Dritte; siehe die [SECURITY.md](https://github.com/KeilerHirsch/schroedinger-sync/blob/main/SECURITY.md) des Tools für das vollständige Threat-Model, inklusive warum „es entschlüsselt einen DPAPI-Cookie-Store" bewusst nicht schöngeredet wird.

**Warum es hierher gehört, nicht nur ins eigene Repo:** es ist eine funktionierende Demonstration von Säule 3s tatsächlicher Messlatte — Persistenz, die eine Session, einen App-Wechsel und die eigenen Anti-Automation-Verteidigungen einer Plattform übersteht, kein Spielzeugbeispiel. Windows-spezifisch, MIT-lizenziert, gebaut mit derselben „nur eigener Account, keine verdeckte Operation, Tests erzwingen die Invarianten"-Disziplin, für die dieses Repo argumentiert.

---

Hast du wegen dieser These etwas gebaut? Öffne ein Issue — siehe [CONTRIBUTING](../../CONTRIBUTING.md).
