# IIoT-Block — Automatisierungstechnik (MCI Mechatronik BSc)

Dieser Block behandelt die Anbindung von Produktionsanlagen an eine Cloud-Infrastruktur über MQTT sowie die anschließende Datenspeicherung, Visualisierung und maschinelle Auswertung der Sensordaten.

Die Daten werden vom Lehrenden über einen Simulator bereitgestellt, der eine Abfüllanlage (Teaching Factory) abbildet. Ihr abonniert die Topics, speichert die Daten und trainiert ML-Modelle darauf.

---

## Aufgaben

| Aufgabe | Inhalt | Gewichtung | Abgabe |
|---------|--------|-----------|--------|
| 12.1.1 | TwinCAT MQTT-Client: Sensordaten der SPS an den Broker publishen | 20% | Termin 2 (12.06) |
| 12.1.2 | Python Subscriber: `iot1/teaching_factory/#` abonnieren, CSV speichern, Zeitreihe visualisieren | 40% | Termin 2–3 |
| 12.3 | scikit-learn Regression: Endgewicht der Flasche vorhersagen | 20% | Termin 4 (26.06) |
| 12.4 | scikit-learn Klassifikation: gesprungene Flaschen erkennen | 20% | Termin 4 (26.06) |

**Projektabgabe:** 03.07.2026

---

## MQTT-Broker

| Parameter | Wert |
|-----------|------|
| Host | `158.180.44.197` |
| Port | `1883` |
| Username | `bobm` |
| Password | `letmein` |

Kein eigener Account nötig — ihr verbindet euch direkt mit diesen Credentials über MQTT Explorer oder paho-mqtt.

---

## Topic-Struktur

### Teaching Factory (Datenquelle für 12.1.2)

Der Lehrende betreibt einen Simulator der kontinuierlich Produktionsdaten publisht. Ihr müsst diesen Simulator **nicht selbst starten**.

| Topic | Inhalt | Beispiel-Payload |
|-------|--------|-----------------|
| `iot1/teaching_factory/recipe` | Rezept der aktuellen Flasche (Farbmengen in Gramm) | `{"id": "42", "creation_date": "2026-05-17", "color_levels_grams": {"red": 8, "blue": 18, "green": 11}}` |
| `iot1/teaching_factory/scale/final_weight` | Endgewicht der Flasche nach dem Befüllen | `{"bottle": "79024867", "time": 1779024879, "final_weight": 35.84}` |
| `iot1/teaching_factory/drop_oscillation` | Schwingungszeitreihe beim Aufsetzen der Flasche (500 Messpunkte) | `{"bottle": "79024867", "drop_oscillation": [0.0, 0.087, ...]}` |
| `iot1/teaching_factory/ground_truth` | Ob die Flasche gesprungen ist | `{"bottle": "79024867", "is_cracked": false}` |

### Eure SPS (für 12.1.1)

Ihr publisht unter eurem eigenen Gruppen-Prefix:

```
aut/<Gruppe>/$groupsname     → Gruppenname (einmalig beim Start, Retain)
aut/<Gruppe>/names           → Nachnamen (einmalig beim Start, Retain)
aut/<Gruppe>/<Größe>         → Messwert (periodisch, alle 10s)
aut/<Gruppe>/<Größe>/$unit   → SI-Einheit (einmalig beim Start, Retain)
```

---

## Abgabe

- **Format:** Git-Repository mit Quellcode + Dokumentation als Markdown-Datei
- **Datum:** 03.07.2026
- **Inhalt:** Quellcode aller Aufgaben, Report mit Plots, Confusion Matrix, Feature-Tabellen

---

## Ordnerstruktur

```
MCI-AUT-IIoT/
├── iot_simulator/          # Vom Lehrenden betrieben — nicht für Studierende
├── 12_1_2_Datenspeicherung/
├── 12_3_Regression/
└── 12_4_Klassifikation/
```
