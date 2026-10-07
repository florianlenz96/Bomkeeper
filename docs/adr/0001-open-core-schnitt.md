# ADR-0001: Open-Core-Schnitt

- **Status:** Akzeptiert (Lizenz des kommerziellen Teils: offen, siehe unten)
- **Datum:** 2026-10-07

## Kontext

Bomkeeper soll Einnahmen erzielen und zugleich Werbung für die Beratung sein. Ein Archiv mit zehnjähriger Aufbewahrung verlangt außerdem besonderes Vertrauen: Kunden fragen zu Recht, was mit ihren Nachweisen passiert, wenn es Bomkeeper nicht mehr gibt.

## Entscheidung

Alles, was ein Kunde braucht, um seine Nachweise **ohne Bomkeeper** zu lesen und zu prüfen, ist offen (Apache-2.0): Archivformat und Spezifikation, CLI, Speicher-Adapter, Verifier, GitHub Action.

Komfort, Struktur und Betrieb sind kommerziell: Server, Produktmodell, Index, Web-UI, Dependency-Track-Anbindung, Freigaben, Audit-Log, bomkeeper.cloud.

## Begründung

- Ein komplett geschlossenes Archiv erzeugt Lock-in-Angst und ist für ein Zehn-Jahres-Produkt ein Kaufhindernis.
- Ein komplett offenes Produkt lässt sich kaum monetarisieren, sbomify besetzt diesen Weg bereits.
- Der offene Kern ist zugleich Marketing: Talks, Blogposts, Community-Beiträge.

## Folgen

- Der offene Teil darf nie von kommerziellem Code abhängen (Labels `oss` / `commercial`).
- Bis Release 0.1 ein privates Monorepo; danach wird der offene Teil in ein öffentliches Repo verschoben.
- **Offen:** Lizenz des kommerziellen Teils – proprietär oder Functional Source License (wird nach zwei Jahren Apache-2.0). Empfehlung FSL. Entscheidung im Issue „Lizenzmodell entscheiden“ (M0).
