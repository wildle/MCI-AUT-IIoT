# Projekt

## Aufgabenübersicht

Im Rahmen des IIoT-Blocks sind vier Aufgaben zu bearbeiten. Die Aufgaben bauen aufeinander auf und ergeben zusammen eine durchgehende Datenpipeline vom Sensor bis zum Machine-Learning-Modell.

| Aufgabe | Inhalt | Gewichtung |
|---------|--------|-----------|
| 12.1.1 | MQTT-Client in TwinCAT SPS — Sensordaten publizieren | 20% |
| 12.1.2 | Python Datenspeicherung & Visualisierung | 40% |
| 12.3 | Regressionsmodell mit scikit-learn | 20% |
| 12.4 | Klassifikationsmodell mit scikit-learn | 20% |

**Abgabedatum: 03.07.2026**

---

## Aufgabe 12.1.1 — MQTT-Client SPS

Implementiert einen MQTT-Client in TwinCAT (`FB_IotMqttClient`), der Sensordaten der Learning Factory alle 10 Sekunden an den Broker sendet.

**Topic-Schema:**

| Topic | Inhalt | Retain | Wann |
|-------|--------|--------|------|
| `aut/SoSe26/<Gruppe>/$groupsname` | Gruppenname | TRUE | einmalig beim Start |
| `aut/SoSe26/<Gruppe>/names` | Nachnamen der Mitglieder | TRUE | einmalig beim Start |
| `aut/SoSe26/<Gruppe>/<Größe>` | Messwert (INT oder REAL) | TRUE | periodisch (10s) |
| `aut/SoSe26/<Gruppe>/<Größe>/$unit` | SI-Einheit als String | TRUE | einmalig beim Start |

**Broker:** `158.180.44.197:1883`, User: `bobm`, Passwort: `letmein`

---

## Aufgabe 12.1.2 — Datenspeicherung & Visualisierung

Schreibt einen Python MQTT-Subscriber, der Daten vom Broker empfängt, in einer Datenbank speichert und eine Zeitreihe visualisiert.

**Mindestanforderung:**

- CSV als Datenbank
- Alle relevanten Topics vollständig gespeichert
- Visualisierung einer beliebigen Zeitreihe (matplotlib)
- Mindestens 15 Minuten Daten gesammelt
- Plot im Report als Screenshot

**Bonus:**

- TinyDB, SQLite oder InfluxDB statt CSV
- Grafana oder Plotly Dash statt matplotlib
- System über config-Datei konfigurierbar (Broker, Topic)
- Fehlerbehandlung (Verbindungsabbruch)
- REST-API oder SQL-Abfragen zum Datenabruf

---

## Aufgabe 12.3 — Regressionsmodell

Trainiert ein lineares Regressionsmodell (scikit-learn) zur Vorhersage des Endgewichts einer Flasche aus den Sensordaten.

**Report enthält:**

- Tabelle: genutzte Features (X) → Zielgröße (y)
- MSE und R² pro Feature-Kombination
- Datei `reg_<Gruppe>.csv` mit den Ergebnissen

---

## Aufgabe 12.4 — Klassifikationsmodell

Trainiert ein Klassifikationsmodell zur Erkennung defekter Flaschen (`is_cracked`) anhand der Vibrationszeitreihe (500 Messpunkte).

**Report enthält:**

- Feature Engineering: statistische Merkmale aus der Zeitreihe (RMS, Mean, STD, Min, Max, Range, Median)
- Confusion Matrix
- Tabelle: genutzte Features + F1-Score pro Kombination
