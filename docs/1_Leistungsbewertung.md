# Leistungsbewertung

Der IIoT-Block wird mit **1/4 der Gesamtnote** gewichtet. Die restlichen 3/4 entfallen auf den Automatisierungstechnik-Teil.

## Gewichtung der Aufgaben

| Aufgabe | Inhalt | Gewichtung (IIoT-Block) |
|---------|--------|------------------------|
| 12.1.1 | MQTT-Client SPS | 20% |
| 12.1.2 | Datenspeicherung & Visualisierung | 40% |
| 12.3 | Regressionsmodell | 20% |
| 12.4 | Klassifikationsmodell | 20% |

---

## Aufgabe 12.1.1 — MQTT-Client SPS (20%)

**Mindestanforderung (volle Punktzahl):**

- Sensordaten werden alle 10 Sekunden an den Broker gesendet
- Topic-Schema wird eingehalten (`$groupsname`, `names`, Messwert, `$unit`)
- Retain-Flag korrekt gesetzt
- Werte sind via MQTT-Explorer am Broker sichtbar

---

## Aufgabe 12.1.2 — Datenspeicherung & Visualisierung (40%)

**Mindestanforderung (volle Punktzahl):**

- CSV als Datenbank
- Alle relevanten Topics vollständig und korrekt gespeichert
- Plot einer Zeitreihe im Report
- Mindestens 15 Minuten Daten gesammelt

**Bonus (Punktzahl erhöhen):**

- TinyDB, SQLite oder InfluxDB statt CSV
- Grafana oder Plotly Dash statt matplotlib
- System über config-Datei konfigurierbar
- Fehlerbehandlung (Verbindungsabbruch)
- REST-API oder SQL-Abfragen zum Datenabruf

---

## Aufgabe 12.3 — Regressionsmodell (20%)

**Mindestanforderung (volle Punktzahl):**

- Lineares Regressionsmodell trainiert und ausgewertet
- Tabelle mit genutzten Features und MSE/R² im Report
- Ergebnisdatei `reg_<Gruppe>.csv` vorhanden

---

## Aufgabe 12.4 — Klassifikationsmodell (20%)

**Mindestanforderung (volle Punktzahl):**

- Feature Engineering aus der Vibrationszeitreihe (statistische Merkmale)
- Klassifikationsmodell trainiert und ausgewertet
- Confusion Matrix im Report
- Tabelle mit genutzten Features und F1-Score

---

## Abgabeformat

Abgabe als **öffentliches GitHub-Repository** mit `README.md` als Hauptdokumentation.

**Abgabedatum: 03.07.2026**
