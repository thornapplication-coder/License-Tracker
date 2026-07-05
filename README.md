# Instructor Overview – Dashboard

Ein eigenständiges Dashboard zur Überwachung von **Lizenz-Ablaufdaten** von
Instruktoren und Prüfern (TRI/TRE/SFI/SFE …). Excel-Datei hochladen — fertig:
alle Ablaufdaten, Warnstufen und Filter auf einen Blick.

Die ganze App ist eine einzige Datei: **`index.html`** (plus die selbst gehosteten
Bibliotheken SheetJS und jsPDF in `vendor/`). Kein Build, kein Server, keine
Installation — einfach im Browser öffnen.

**Reiner Converter — standardmäßig keine Datenspeicherung:** Es gibt **keinen
Server**; alle Verarbeitung passiert lokal im Browser. Standardmäßig werden
**keine Lizenzdaten gespeichert** — beim Schließen oder Neuladen ist alles weg,
und es wird jeweils die aktuelle Excel-Liste frisch hochgeladen. Nur die
Benutzereinstellungen (Sprache, Warnschwellen, Spaltenzuordnung) bleiben erhalten.

**Optionale lokale Speicherung (Standard: AUS):** In den Einstellungen kann
bewusst aktiviert werden, dass die zuletzt geladene Liste **unverschlüsselt im
`localStorage` dieses Browsers/Geräts** behalten wird (Schlüssel `ew-ilc-data-v1`),
sodass sie ein Neuladen übersteht. Ist die Option aktiv, zeigt das Kopf-Badge
„💾 Lokal gespeichert"; sie lässt sich in den Einstellungen jederzeit abschalten
und die Daten mit einem Klick löschen. Nur auf privaten, vertrauenswürdigen
Geräten verwenden.

> **Hinweis:** Dieser Ordner ist absichtlich vollständig unabhängig vom Rest des
> Repositories. Zum Umzug in ein eigenes Repository einfach den kompletten Ordner
> `instructor-license-dashboard/` verschieben — es gibt keine Abhängigkeiten nach außen.
> Für GitHub Pages den Ordnerinhalt als Repo-Root veröffentlichen.

## Features

- **Eigene Upload-Seite** mit großer Drop-Zone (Startseite ohne Daten; über
  „Excel hochladen" jederzeit erreichbar, mit Hinweis, dass ein neuer Upload
  die geladenen Daten ersetzt).
- **Excel-Upload** (`.xlsx`, `.xls`, `.xlsm`, `.xlsb`, `.csv`) per Button oder Drag & Drop.
  Kopfzeile, Namensspalten und **Datumsspalten werden automatisch erkannt**
  (auch bei Titelzeilen über der Kopfzeile). Excel-Datumszellen, Serienwerte und
  Textdaten (`31.12.2026`, `2026-12-31`, `31/12/2026`) werden verstanden.
- **Mehrere Tabs (Arbeitsblätter)**: Jedes Blatt der Arbeitsmappe wird eingelesen und
  ist im Dashboard als eigener Reiter umschaltbar (KPIs, Warnungen und Tabelle jeweils
  pro Reiter — Archiv-Tabs wie „ausgeschiedene …" verfälschen die aktiven Zahlen nicht).
- **Zweizeilige Kopfzeilen** (z. B. Flugzeugmuster über TRI/SFI/TRE-Spalten, auch mit
  verbundenen Zellen) werden zu eindeutigen Spaltennamen kombiniert („C560 XLS · TRI").
  Der Prefix wird nur gesetzt, wo die Unterspalte sonst mehrdeutig wäre.
- **Vor- und Nachname** (Firstname/Surname bzw. Vorname/Nachname) werden in
  Warnliste und Anzeige automatisch zu einem Namen zusammengeführt.
- **„Nur Spalten mit Warnungen"**-Schalter: blendet bei sehr breiten Tabellen alle
  Spalten ohne fällige/abgelaufene Einträge aus.
- **Referenzdatum-Leiste**: Das Dashboard zeigt prominent das Referenzdatum an —
  immer das **tagesaktuelle Datum** (live aus der Systemuhr, nie hartkodiert).
  Alle Ablauffristen werden dagegen berechnet. Bleibt der Tab über Mitternacht
  offen, aktualisiert sich die Ansicht automatisch auf den neuen Tag.
- **Dashboard mit KPI-Kacheln**: Personen, Lizenzeinträge, Abgelaufen, Kritisch,
  Warnung, Gültig. Kacheln sind klickbar und filtern die Tabelle.
- **Warnhinweise**: abgelaufene und bald fällige Lizenzen, nach Datum sortiert,
  mit „läuft in X Tagen ab“ / „vor X Tagen abgelaufen“.
- **Warnschwellen einstellbar** (Standard: kritisch ≤ 30 Tage, Warnung ≤ 90 Tage).
- **Status-Schnellfilter**: Chip-Leiste (Abgelaufen / Kritisch / Warnung / Gültig /
  Kein Datum) mit Personen-Zählern, **Mehrfachauswahl**; filtert Warnliste **und**
  Tabelle. KPI-Kacheln schalten dieselben Chips. **Standard nach dem Upload:
  „Abgelaufen" aktiv** — „Filter zurücksetzen" zeigt alles.
- **Monatskalender**: Kalenderansicht mit ⛔ abgelaufenen und 🔴 kritischen
  Fälligkeiten pro Tag (Monatsnavigation, „Heute"-Knopf, Tagesklick zeigt die
  Einträge). Ein Klick exportiert genau diese Termine als **.ics in den eigenen
  Kalender** (mit Erinnerungen 30/7 Tage vorher).
- **DE/EN-Umschalter** und **↻-Reload-Knopf** im Kopfbereich (Reload fragt nach,
  wenn Daten geladen sind). Beim Öffnen nach einem App-Update erscheint eine
  **Update-Notification** mit der neuen Versionsnummer.
- **„Nur Aktive"-Schalter** in der Schnellfilter-Leiste (erscheint, wenn der Tab eine
  „Aktiv"-Spalte hat): KPIs, Warnliste und Tabelle rechnen dann nur mit Personen,
  die in der Aktiv-Spalte markiert sind — Altlasten inaktiver Einträge verschwinden.
- **Personen-Detailansicht**: Klick auf einen Namen öffnet eine Karte — zuerst
  **Personal Details** in fester Reihenfolge (Firstname, Surname, Pilot Lic.Nr.,
  Mail AAA, Mail privat, Telephone, Adresse, HomeBase), dann alle
  Lizenzen/Ablaufdaten (sortiert, mit Status) und die weiteren Angaben.
- **Fixierte Spalten**: Löschen-, Status- und Namensspalten bleiben beim
  horizontalen Scrollen durch breite Tabellen links stehen.
- **Datenqualitäts-Bericht** (alle Tabs): findet ungültige Datumswerte
  (z. B. „31.02.2024"), mögliche Duplikate (gleicher Name) und Zeilen ohne Namen —
  als Dialog und CSV-Export zum Bereinigen der Quelldatei.
- **Passwortgeschützte Excel-Dateien**: verschlüsselte Arbeitsmappen werden nach
  Passwort-Eingabe **direkt im Browser** entschlüsselt (selbst gehostete
  xlsx-populate-Bibliothek, lazy geladen). Das Passwort wird nie gespeichert.
- **Suche + Filter**: Volltextsuche, automatische Wertfilter für Textspalten
  (z. B. Aktiv, Funktion — inkl. „(leer)"-Option); sortierbare Spalten.
- **Refresh-Button**: Stichtag „heute“ wird neu berechnet und alles neu bewertet.
- **JSON-Backup** (optional): Momentaufnahme als Datei herunterladen und später
  wieder laden — z. B. um einen Zwischenstand ohne die Original-Excel weiterzugeben.
  Backups werden beim Import **gegen ein Schema validiert** (Whitelist, Typprüfung,
  Größenlimits). Gespeichert wird dabei nichts — es sind reine Dateien.
- **Spalten-Editor**: falsch erkannte Spalten manuell auf Text/Datum umstellen;
  die Zuordnung wird für künftige Importe derselben Excel-Struktur gemerkt.
- **Personen löschen**: 🗑 pro Zeile mit Bestätigungs-Dialog und **Rückgängig**-Option;
  gelöschte Personen erscheinen nicht mehr im Dashboard (nur für die aktuelle
  Sitzung — der nächste Upload bringt wieder den vollen Excel-Stand).
- **Export** der aktuellen Ansicht (Tab + Suche/Filter werden angewendet) mit
  **Status-Auswahl im Export-Dialog** (Checkboxen: abgelaufen, kritisch, Warnung,
  gültig, kein Datum — vorbelegt aus dem Schnellfilter, mit Live-Zeilenzähler):
  - **PDF-Report** (Querformat, paginiert): übersichtliche Warnliste mit Status,
    Person, Lizenz, Ablaufdatum und Resttagen.
  - **Excel (.xlsx)**: alle Spalten inkl. Status, mit **echten Datumszellen**.
  - **CSV** (Semikolon, UTF-8 mit BOM — direkt Excel-kompatibel).
  - **Kalender (.ics)**: ein Termin pro Ablaufdatum mit Erinnerungen 30 und 7 Tage
    vorher — einmal in Outlook & Co. importieren und aktiv erinnert werden.
- **Export pro Person**: Die Personen-Detailansicht bietet dieselben Formate
  (PDF, Excel, CSV, iCal, E-Mail) nur für diese eine Person — z. B. um jemandem
  seine eigenen Fälligkeiten zu schicken.
- **Versionsvergleich**: Ältere Excel-Version hochladen → Änderungsbericht gegen
  den aktuell geladenen Stand: neue/entfernte Personen und geänderte Datumswerte
  (verlängert/verkürzt/neu/entfernt), pro Tab, mit CSV-Export. Auch die alte Datei
  darf passwortgeschützt sein.
- **E-Mail-Report**: öffnet das eigene Mailprogramm (mailto) mit fertigem Betreff
  und der Warnliste als Text — Statusauswahl wie beim Export, Empfänger werden
  gemerkt (ab Werk vorbelegt mit den AAA-Standardadressen). Für lange Listen: „Text kopieren" (voller Report in die Zwischenablage)
  und PDF-/Excel-Export als Anhang (mailto kann technisch keine Anhänge).
- **Daten löschen** mit getippter Bestätigung und **automatischem Backup davor**.
- **Druck-/PDF-Report** (ein Print-Button, druckoptimiertes Layout).
- **DE/EN-Sprachumschaltung**, `Intl`-Datumsformatierung.
- **Statusanzeige nie nur über Farbe** (immer Icon + Text), Modals mit Focus-Trap
  und Esc, ehrliche Speicher-Rückmeldung (✓/⚠ + Auto-Backup bei Speicherfehler).

## Statuslogik

| Status | Bedingung (Tage bis Ablauf) |
|---|---|
| ⛔ Abgelaufen | < 0 |
| 🔴 Kritisch | 0 … kritische Schwelle (Standard 30) |
| ⚠️ Warnung | … Warn-Schwelle (Standard 90) |
| ✅ Gültig | > Warn-Schwelle |

Der Status einer Person ist immer der **schlechteste** Status all ihrer Datumsspalten.

## Erwartetes Excel-Format

Flexibel — es gibt keine Pflichtstruktur. Empfohlen:

| Name | Funktion | Basis | Lizenz Ablauf | Medical Ablauf | … |
|---|---|---|---|---|---|
| Muster, Max | TRI | DUS | 31.12.2026 | 15.06.2026 | … |

Jede Spalte, die überwiegend Daten enthält, wird als Ablaufdatum überwacht.
Beliebig viele Datumsspalten sind möglich. Da immer dieselbe Excel-Struktur
hochgeladen wird, bleibt die (ggf. manuell korrigierte) Spaltenzuordnung erhalten.

## Sicherheit & Datenschutz

- Läuft komplett clientseitig; keine Datenübertragung an einen Server.
- **Standardmäßig keine Persistenz von Lizenzdaten** — Daten existieren nur im
  Arbeitsspeicher des offenen Tabs (ideal für geteilte Rechner). Optional lässt
  sich in den Einstellungen die lokale Speicherung aktivieren; dann liegt die
  Liste **unverschlüsselt** im `localStorage` dieses Geräts, bis sie dort
  deaktiviert/gelöscht wird (siehe oben).
- Content-Security-Policy per `<meta>`; SheetJS, jsPDF und xlsx-populate selbst
  gehostet (kein CDN).
- Alle Nutzerdaten werden beim Rendern HTML-escaped (inkl. `"` und `'`).

## Entwicklung

- Vor jedem Push: Inline-Script extrahieren und `node --check` ausführen.
- E2E-Test: App per `python3 -m http.server` servieren und mit Playwright
  (Chromium) Upload/Filter/Backup/Restore durchklicken.
