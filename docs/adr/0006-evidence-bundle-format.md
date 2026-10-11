# ADR-0006: Versioniertes, portables Evidence-Bundle-Format

- **Status:** Vorgeschlagen
- **Datum:** 2026-10-11
- **Issue:** #8

## Kontext

Ein Evidence Bundle muss die Original-SBOM und alle Informationen enthalten,
die für seine spätere Prüfung nötig sind. Es muss ohne Bomkeeper, ohne
Netzwerkzugriff und mit allgemein verfügbaren Werkzeugen lesbar bleiben.
Das Bundle ist die fachliche Wahrheit (ADR-0002); seine Inhalte werden nach
dem Archivieren weder geändert noch gelöscht (ADR-0003). Die SBOM darf beim
Archivieren nicht geparst, normalisiert oder neu serialisiert werden.

Ein einzelnes SBOM-Objekt in einem WORM-Speicher erfüllt diese Anforderungen
nicht: Es enthält weder zwingend den Bezug zum Release noch einen
selbstbeschreibenden Integritätsnachweis oder eine portable Prüfanleitung.

## Entscheidung

Das offene Format soll als versionierter, selbstbeschreibender Verzeichnisbaum
spezifiziert werden. Dieses ADR ist ein Vorschlag zur Überprüfung; es legt
noch keine Implementierungsdetails oder Bibliotheken fest.

- `manifest.json` enthält die Formatversion, Release-Metadaten sowie ein
  Inventar aller Nutzdaten mit relativem Pfad, Rolle, Größe und SHA-256-Hash.
- Die originale SBOM und optionale Begleitdokumente liegen als separate Dateien
  im Bundle. Die SBOM wird byte-genau gespeichert; Hashes werden über die
  tatsächlich archivierten Bytes gebildet.
- Signatur und Zeitstempel liegen als separate, standardisierte Nachweisdateien
  bei. Ihre genaue Profile werden in ADR-0007 vorgeschlagen.
- Das logische Bundle ist unabhängig von seiner Ablage: Speicheradapter dürfen
  die Dateien einzeln unter einem Bundle-Präfix ablegen. Der Verzeichnisbaum
  bleibt die normative Struktur; zusätzliche Transportarchive sind kein
  alternatives Format.
- Manifest und Dateiinventar verwenden UTF-8 und einen festgelegten,
  versionierten Schema-Stand. Signiert werden die exakten Manifest-Bytes;
  Verifier dürfen sie nicht vor der Signaturprüfung umformatieren. Doppelte
  JSON-Schlüssel und unsichere bzw. mehrdeutige Pfade werden zurückgewiesen.
- Formatänderungen sind additiv. Ein neues Feld ändert nie die Bedeutung
  bestehender Felder; nicht unterstützte erforderliche Versionen oder
  Erweiterungen führen zu einer klaren Fehlermeldung statt zu einer
  stillschweigenden Teilprüfung.

## Begründung

- Ein Verzeichnisbaum mit gewöhnlichen Dateien braucht kein proprietäres
  Containerformat und lässt sich mit Standardwerkzeugen inspizieren.
- Ein separates Manifest macht Bundle-Inhalt, Release-Bezug und Hashes
  selbstbeschreibend, ohne die SBOM zu verändern.
- Exakte Manifest-Bytes als Signaturgegenstand vermeiden unterschiedliche
  JSON-Kanonisierungen in unabhängigen Implementierungen.
- Die logische Trennung von Bundle und Ablage erlaubt identische Offline-Prüfung
  für lokalen Export, S3, Azure oder andere Speicher.

Ein ZIP- oder TAR-Container als normatives Format wurde wegen zusätzlicher
Komplexität bei Pfaden, Metadaten, Extraktion und langfristiger
Werkzeugkompatibilität nicht bevorzugt. Ein bloßer SBOM-Upload wurde verworfen,
weil er Release-Metadaten und Nachweise nicht zuverlässig mit der Originaldatei
verknüpft.

## Folgen

- Die Formatspezifikation muss Struktur, Pflichtfelder, Dateipfade,
  Fehlerverhalten, Versionsregeln und Beispiele festlegen.
- Referenz-Fixtures müssen gültige Bundles, unbekannte additive Felder und
  fehlerhafte bzw. manipulierte Bundles abdecken.
- Der Verifier liest ausschließlich das Bundle und lokale Vertrauensrichtlinien;
  er benötigt keinen Bomkeeper-Server und kein Netzwerk.
- Die Implementierung des WORM-Speicheradapters ist eine eigene Verantwortung:
  Er setzt die Dateien des Bundles unveränderbar ab, ersetzt aber weder
  Manifest noch Integritätsnachweis.
- Für Epic #8 wird folgende Abgrenzung zur Überprüfung vorgeschlagen:
  - **Archivformat:** offene, versionierte Spezifikation und portable Fixtures.
  - **Integritätsnachweis:** standardbasierte Signatur, Zeitstempel und
    unabhängiger Offline-Verifier (ADR-0007).
  - **WORM-Speicherung:** separate Adapter-Story für unveränderbare Ablage und
    Aufbewahrungsfristen; sie ist kein Ersatz für die ersten beiden Stories.
- Als überarbeiteter Epic-Text wird vorgeschlagen:

  > **Ziel:** Ein offenes, versioniertes Evidence Bundle, das Original-SBOM,
  > Release-Bezug und Integritätsnachweise selbstbeschreibend enthält und in
  > zehn Jahren ohne Bomkeeper oder Netzwerk gelesen und geprüft werden kann.
  >
  > **Stories:** Spezifikation des portablen Archivformats; kryptografischer
  > Integritätsnachweis mit unabhängigem Offline-Verifier; separate
  > WORM-Adapter-Story für unveränderbare Ablage und Aufbewahrungsfristen.
  >
  > **Abnahme:** Ein unabhängiger Dritter kann anhand der veröffentlichten
  > Spezifikation ein Bundle ohne Bomkeeper lesen. Die archivierte SBOM stimmt
  > byte-genau mit ihrer Quelle überein. Eine portable Fixture lässt sich
  > offline erfolgreich prüfen; gezielte Änderungen an Nutzdaten oder
  > Nachweisen werden zuverlässig erkannt. WORM-Retention wird separat durch
  > Adaptertests nachgewiesen und nicht als Ersatz für kryptografische Prüfung
  > gewertet.
