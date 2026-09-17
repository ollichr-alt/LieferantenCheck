# team LieferantenCheck – Mobile Core 0.5

## Ziel dieses Stands
Die App ist jetzt für große reale Datenpakete vorbereitet. Unternehmensdaten werden nicht mehr als eine einzige riesige JSON gedacht, sondern als segmentiertes Paket.

## Segmentiertes Datenpaket
Ordnerstruktur:

```text
LieferantenCheck_Data_2026-09-17/
├── manifest.json
├── suppliers.jsonl
├── locations.jsonl
├── articles.jsonl
├── stock.jsonl
├── sales.jsonl
└── aggregates.jsonl
```

Jedes Segment hat im Manifest:
- Typ
- Dateiname
- Format
- erwartete Zeilenzahl
- SHA-256

## Import-Sicherheit
1. Manifest prüfen
2. alle Pflichtsegmente prüfen
3. jeden Datei-Hash prüfen
4. JSONL validieren
5. erst danach lokale Datenbank ersetzen
6. bei Fehler bleibt der bisherige Datenstand aktiv

## v5.1.91 Converter
`tools/convert_v5191.py`

Der Converter erwartet zunächst einen normalisierten Export aus v5.1.91 und erzeugt daraus den segmentierten Datenordner.

Beispiel:

```bash
python3 tools/convert_v5191.py v5191-normalized.json ./LieferantenCheck_Data
```

## Warum JSONL?
Bei großen Datenmengen können Segmente einzeln verarbeitet werden. Das vermeidet eine einzige gigantische JSON-Datei und erleichtert spätere Streaming-/Delta-Updates.

## Test
Unter `sample-data/segmented-demo/` liegt ein vollständiges Mini-Datenpaket.

In der App:
1. `Datenpaket wählen`
2. den Ordner `segmented-demo` auswählen
3. Import verfolgen
4. danach `problemkapital` im Assistant testen

## Nächste Stufe
- echter Export-Adapter direkt aus v5.1.91
- segmentweises Streaming ohne komplettes Segment im RAM
- Suchindex als eigener Segmenttyp
- Delta-Pakete
- Recovery Snapshot vor Live-Switch
- Dashboard / Lieferanten / Standortseiten auf echter Datenbasis
