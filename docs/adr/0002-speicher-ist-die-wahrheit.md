# ADR-0002: Der Speicher ist die Wahrheit

- **Status:** Akzeptiert
- **Datum:** 2026-10-07

## Kontext

Bomkeeper muss Nachweise zehn Jahre und länger halten. In dieser Zeit werden Datenbanken migriert, Server neu aufgesetzt und vielleicht der Betreiber gewechselt. Bomkeeper soll außerdem ohne Server nutzbar sein (Lite-Modus).

## Entscheidung

Die Evidence Bundles im unveränderbaren Objektspeicher sind die einzige fachliche Wahrheit. Datenbank und Suchindex sind Cache und lassen sich jederzeit mit `reindex` allein aus den Bundles wiederherstellen.

## Begründung

- WORM-Speicher (S3 Object Lock, Azure Immutable Blob) ist für Aufbewahrungspflichten gebaut; Datenbanken sind es nicht.
- Backup und Wiederherstellung werden trivial: Speicher sichern genügt.
- Bring-your-own-Storage und Lite-Modus funktionieren nur, wenn alle nötigen Informationen im Bundle stehen.

## Folgen

- Jede fachliche Information (Produkt, Release, Inverkehrbringen, Unterstützungsende, Zuordnung von Begleitdokumenten) muss im Manifest eines Bundles stehen, nicht nur in der Datenbank.
- Nachträgliche Korrekturen werden als **neues** Bundle geschrieben, das auf das alte verweist – nie als Änderung.
- Jede neue Funktion braucht einen Test, dass `reindex` ihren Zustand wiederherstellt.
