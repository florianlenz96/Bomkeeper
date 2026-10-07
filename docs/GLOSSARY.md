# Glossar

Begriffe, die in Code, Issues und Doku einheitlich verwendet werden. Englischer Begriff in Klammern = Name im Code.

| Begriff | Bedeutung |
| --- | --- |
| **SBOM** (Software Bill of Materials) | Maschinenlesbare Stückliste der Komponenten eines Softwareprodukts. Unterstützte Formate: CycloneDX, SPDX. |
| **Evidence Bundle** (`Bundle`) | Die archivierte Einheit von Bomkeeper: Original-SBOM (byte-genau), `manifest.json` mit Metadaten, SHA-256-Hash, Signatur, RFC-3161-Zeitstempel, optional Begleitdokumente. Einmal geschrieben, nie geändert. |
| **Manifest** (`Manifest`) | Metadaten eines Bundles: Produkt, Version, Release, Commit, Build, Hersteller, Format, Inverkehrbringen, Unterstützungsende. |
| **Produkt** (`Product`) | Ein Produkt mit digitalen Elementen im Sinne des CRA, z. B. „Steuerung X200“. |
| **Version** (`Version`) | Eine Produktversion, z. B. „3.2“. |
| **Release** (`Release`) | Ein konkret ausgelieferter Stand einer Version, z. B. „3.2.1, Build 4711“. Daran hängen Bundles. |
| **Inverkehrbringen** (`placedOnMarketAt`) | Erstmaliges Bereitstellen eines Produkts auf dem EU-Markt. Startpunkt der zehnjährigen Aufbewahrungspflicht. |
| **Unterstützungszeitraum** (`supportEndsAt`) | Zeitraum, in dem der Hersteller Schwachstellen behandeln muss; nach CRA in der Regel mindestens fünf Jahre. |
| **Aufbewahrungsfrist** (`retainUntil`) | Späteres Datum aus (Inverkehrbringen bzw. Bereitstellung + 10 Jahre) und Unterstützungsende. Darf nur verlängert, nie verkürzt werden. |
| **Technische Dokumentation** | Nach CRA vorzuhaltende Unterlagen eines Produkts, darunter die SBOM, Risikobewertung und EU-Konformitätserklärung. |
| **Begleitdokument** (`Attachment`) | Weiteres Nachweisdokument im Bundle, z. B. EU-Konformitätserklärung, Risikobewertung, VEX, Release Notes. |
| **WORM** (Write Once, Read Many) | Speicher, der Daten bis Fristende gegen Ändern und Löschen sperrt. Bei S3: Object Lock im Compliance-Modus. Bei Azure: gesperrte zeitbasierte Immutability-Richtlinie. |
| **Legal Hold** | Zusätzliche, unbefristete Sperre, z. B. für einen Rechtsstreit. Wird protokolliert. |
| **Lite-Modus** | Nutzung nur mit CLI und eigenem Bucket, ohne Bomkeeper-Server. |
| **Bring-your-own-Storage (BYOS)** | bomkeeper.cloud verwaltet, die Bundles liegen aber im Speicher des Kunden. Bomkeeper hat dort kein Löschrecht. |
| **Freigabe** (`Share`) | Zeitlich begrenzter, protokollierter Zugriff für Auditor, Behörde oder Kunde auf ausgewählte Releases. |
| **Nachweispaket** (`EvidenceExport`) | Export für die Marktüberwachung: Bundles, Begleitdokumente, Signaturen, Zeitstempel und Prüfanleitung. |
| **Reindex** | Neuaufbau von Datenbank und Suchindex allein aus den Bundles im Speicher. |
| **Replay** | Erneutes Einspielen archivierter SBOMs in Dependency-Track. |
| **PURL** (Package URL) | Standardisierte Kennung einer Komponente, z. B. `pkg:maven/org.apache.logging.log4j/log4j-core@2.14.1`. Grundlage der Komponentensuche. |
| **VEX** | Vulnerability Exploitability eXchange: Aussage, ob eine Schwachstelle ein Produkt tatsächlich betrifft. |
| **Dependency-Track (DT)** | OWASP-Plattform zur laufenden Schwachstellenanalyse von SBOMs. Bomkeeper speist DT, ersetzt es nicht. Zielversion: v5. |
| **Marktüberwachungsbehörde** | Behörde, die die Einhaltung des CRA prüft; in Deutschland das BSI. |
| **CRA** | Cyber Resilience Act, Verordnung (EU) 2024/2847. |
