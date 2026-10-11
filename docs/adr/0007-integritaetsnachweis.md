# ADR-0007: Kryptografischer Integritätsnachweis und WORM

- **Status:** Vorgeschlagen
- **Datum:** 2026-10-11
- **Issue:** #8

## Kontext

Ein prüfbares Archiv muss nachweisen können, dass die gespeicherten Bytes
unverändert sind und zu den signierten Release-Angaben gehören. Das ist
unabhängig davon nötig, ob die Ablage WORM unterstützt. Ein WORM-Speicher
verhindert im Rahmen seiner Konfiguration bestimmte Änderungen und Löschungen
am Speicherobjekt; er beweist für sich genommen weder, wer die SBOM erstellt
oder eingereicht hat, noch dass sie bereits beim Archivieren korrekt war.
Außerdem schützt die WORM-Sperre nicht notwendigerweise eine exportierte Kopie
oder eine Kopie außerhalb dieses Speichers.

Der Nachweis muss auch nach Ablauf der Aufbewahrungszeit des Speichers und ohne
Verbindung zu einem Bomkeeper-Dienst offline prüfbar sein. Eine Signatur belegt
die Zuordnung zu einem Schlüssel, aber Vertrauen in diesen Schlüssel hängt von
einer separat festgelegten Vertrauensrichtlinie ab. Ein Zeitstempel belegt
einen Zeitpunkt gemäß dem Vertrauen in den Zeitstempeldienst; er belegt nicht
die Wahrheit der signierten Metadaten.

## Entscheidung

Jedes Bundle soll eine offline prüfbare, standardbasierte Nachweiskette
enthalten: Datei-Hashes im Manifest, eine abgetrennte Signatur über die exakten
Manifest-Bytes und einen RFC-3161-Zeitstempel über die Signatur. Diese
Entscheidung ist vorgeschlagen und soll vor Implementierung überprüft werden.

- Das Manifest enthält SHA-256-Hashes und Größen für die originale SBOM und
  jedes Begleitdokument. Der Verifier berechnet die Hashes erneut über die
  gespeicherten Bytes.
- Eine abgetrennte CMS-Signatur (CMS SignedData) signiert die exakten Bytes von
  `manifest.json`; der öffentliche Signaturschlüssel und benötigte
  Zertifikatskette werden im Signaturnachweis mitgeführt. Der private Schlüssel
  verbleibt außerhalb des Bundles.
- Der RFC-3161-Zeitstempel bezieht sich auf den Signaturwert. Der Verifier
  validiert Signatur und Zeitstempel offline gegen explizit konfigurierte
  Vertrauensanker und Richtlinien. Mitgeführte Zertifikate sind Prüfdaten,
  aber nicht automatisch Vertrauensanker.
- Ein erfolgreicher Verifier-Ausgang unterscheidet mindestens die Ergebnisse
  „Bytes und Signatur stimmen“, „Zeitstempel gültig“ und „Signaturkette gemäß
  lokaler Richtlinie vertrauenswürdig“. Fehlende Vertrauensanker, nicht
  verfügbare Revokationsinformationen oder ein nicht unterstütztes Profil
  dürfen nicht als vollständige Vertrauensprüfung ausgegeben werden.
- Das kryptografische Profil wird versioniert. Algorithmen, Schlüsselgrößen,
  Zertifikatsprüfung, Revokationsnachweise und eine spätere Migration auf
  stärkere Profile müssen in der Formatspezifikation festgelegt werden. Es
  werden keine eigenen kryptografischen Verfahren entworfen.
- Die WORM-Integration ist eine zusätzliche Schutzschicht: Adapter müssen
  Retention und Schreib-/Löschschutz gemäß ADR-0003 durchsetzen und ihren
  Status separat ausweisen. WORM-Status wird nicht mit erfolgreicher
  kryptografischer Verifikation gleichgesetzt.

## Ablaufdiagramm

Das Diagramm zeigt getrennt das Erstellen und Speichern des Bundles sowie die
spätere Offline-Prüfung. Die Syntax kann in sequencediagram.org eingefügt
werden.

```text
title: Evidence Bundle erstellen und offline prüfen
participant Submitter
participant CLI
participant Signer
participant TSA
participant WORM
participant Auditor
participant Verifier
participant TrustPolicy

Submitter->CLI: Original-SBOM und Release-Metadaten
CLI->CLI: Nutzdaten hashen und Manifest-Bytes erzeugen
CLI->Signer: Exakte Manifest-Bytes signieren
Signer-->CLI: Abgetrennte CMS-Signatur
CLI->TSA: RFC-3161-Zeitstempel für den Signaturwert anfordern
TSA-->CLI: Zeitstempel-Token
CLI->WORM: Bundle-Dateien mit Retention ablegen
WORM-->CLI: Ablage- und Retention-Status

Auditor->Verifier: Bundle und lokale Vertrauensrichtlinie bereitstellen
Verifier->Verifier: Hashes, CMS-Signatur und Zeitstempel offline prüfen
Verifier->TrustPolicy: Signatur- und TSA-Kette gegen Vertrauensanker prüfen
TrustPolicy-->Verifier: Vertrauensstatus gemäß lokaler Richtlinie
Verifier-->Auditor: Integritäts-, Zeitstempel- und Vertrauensstatus getrennt ausgeben
```

## Begründung

Die Hashes verbinden die tatsächlichen Nutzdaten mit dem Manifest; die
Signatur bindet dieses Manifest an einen Schlüssel; der Zeitstempel liefert
einen unabhängig prüfbaren Zeitbezug. Zusammen mit lokal vorgegebenen
Vertrauensankern ist die Prüfung ohne Bomkeeper-Dienst möglich.

CMS und RFC 3161 sind etablierte, dokumentierte Formate, für die es
unabhängige Werkzeuge gibt. Ein proprietäres Signaturformat oder eine Prüfung,
die ausschließlich Bomkeeper-Schlüssel bzw. Server benötigt, würde die
langfristige Exit-Garantie gefährden. WORM bleibt wichtig, weil es das
nachträgliche Ersetzen oder Löschen im konfigurierten Speicher erschwert bzw.
verhindert; es beantwortet aber andere Fragen als die kryptografische Kette.

## Folgen

- Der offene Verifier und die Spezifikation müssen kryptografische
  Prüfergebnisse und Speicher-Retention getrennt darstellen.
- Offline-Fixtures müssen sowohl valide Nachweisketten als auch veränderte
  SBOM-, Manifest-, Signatur- und Zeitstempeldateien sowie fehlende oder nicht
  vertrauenswürdige Prüfdaten abdecken.
- Vertrauensanker und Vertrauensrichtlinien werden außerhalb des Bundles
  bereitgestellt und müssen unabhängig verwaltet werden. Das Bundle bleibt
  ohne Netzwerk lesbar und kryptografisch prüfbar, kann aber ohne passende
  lokale Vertrauensrichtlinie keine Identität als vertrauenswürdig einstufen.
- Schlüsselverwaltung, Rotation, Signaturzeitpunkt, Langzeitvalidierung und
  Revokationsnachweise benötigen vor Produktivbetrieb eine konkrete
  Betriebsspezifikation; ein mitgeführtes Zertifikat allein löst diese Fragen
  nicht.
- WORM-Adaptertests prüfen Retention und Ablehnung von Änderungen bzw.
  Löschungen. Verifier-Tests prüfen Hashes, Signaturen, Zeitstempel und
  Vertrauensrichtlinien. Keine dieser Testklassen ersetzt die andere.
- Ein WORM-gespeichertes SBOM allein kann vor Änderungen im jeweiligen Speicher
  schützen. Erst das Evidence Bundle samt Signatur- und Zeitstempelnachweis
  macht Inhalt, Release-Bezug und Integritätsprüfung unabhängig von diesem
  Speicher und Bomkeeper transportierbar.
