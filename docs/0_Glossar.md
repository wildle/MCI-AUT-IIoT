# Glossar IIoT

Nachschlagewerk der wichtigsten Fachbegriffe des Kurses.

---

## IIoT & Industrie 4.0

**Industrie 4.0 / IIoT** — *Industrie 4.0* ist die vierte industrielle Revolution: Digitalisierung und Vernetzung der Produktion durch cyber-physische Systeme. *IIoT* (Industrial Internet of Things) ist die Teilmenge davon, in der Sensoren, Maschinen und Systeme über das Internet vernetzt sind und Daten austauschen.

**Automatisierungspyramide** — Hierarchisches Modell industrieller Steuerungssysteme mit 5 Ebenen: Feld-, Steuerungs-, Prozessleit-, Betriebsleit- und Unternehmensebene. Die Datenmenge nimmt nach oben ab. Im IIoT wird diese strikte Hierarchie aufgebrochen — Geräte kommunizieren auch direkt mit der Cloud.

**Feldebene** — Unterste Ebene: Sensoren (messen physikalische Größen) und Aktoren (greifen in den Prozess ein), angebunden über Feldbusse oder analoge Signale.

**Steuerungsebene** — Echtzeitsteuerung einzelner Anlagen durch **SPS** (Speicherprogrammierbare Steuerung / PLC), programmiert nach IEC 61131-3 (z.B. Structured Text). TwinCAT ist eine SPS-Laufzeitumgebung von Beckhoff.

**Prozessleitebene** — Überwachung und Steuerung mehrerer Anlagen über **SCADA**-Systeme: Visualisierung, Alarmierung, Datenaufzeichnung.

**Betriebsleitebene** — Planung und Steuerung der gesamten Produktion über **MES** (Manufacturing Execution System): Auftragssteuerung, Qualitätssicherung; verbindet Fertigung mit der Unternehmensplanung.

**Unternehmensebene** — Oberste Ebene: unternehmensweite Planung über **ERP** (Enterprise Resource Planning, z.B. SAP) — Finanzen, Einkauf, Logistik, Personal. Keine Echtzeitanforderungen.

**Predictive Maintenance** — Vorausschauende Wartung: Sensordaten (Vibration, Temperatur, Stromaufnahme) werden analysiert, um Ausfälle vorherzusagen, **bevor** sie auftreten — kein ungeplanter Stillstand, keine unnötigen Wartungsarbeiten.

**ETL** — *Extract, Transform, Load*: dreistufiger Datenintegrationsprozess — Daten aus Quellen auslesen (Extract), bereinigen/aufbereiten (Transform), ins Zielsystem schreiben (Load).

---

## Netzwerk & Protokolle

**Protokoll** — Vereinbarter Satz von Regeln und Formaten, der festlegt, wie zwei Systeme miteinander kommunizieren (welche Nachrichten in welcher Reihenfolge, wie Fehler behandelt und Daten kodiert werden). Beispiele: HTTP, MQTT, TCP, IP.

**Stack (Protokollstapel)** — Gesamtheit der übereinander geschichteten Protokolle, die zusammen eine Kommunikation ermöglichen. Jede Schicht nutzt die Dienste der darunterliegenden und bietet Dienste für die darüberliegende an.

**OSI-Schichtenmodell** — Referenzmodell mit 7 Schichten (von Bitübertragung bis Anwendung) zur Beschreibung von Netzwerkkommunikation. In der Praxis wird oft das vereinfachte TCP/IP-Modell mit 4 Schichten verwendet.

**TCP/IP** — Protokollstapel, der das Internet trägt. **TCP** garantiert die vollständige und korrekte Ankunft von Daten (verbindungsorientiert), **UDP** liefert schneller, aber ohne Garantie. **IP** übernimmt Adressierung und Routing.

**TLS-Verschlüsselung** — *Transport Layer Security*, Protokoll zur Verschlüsselung von Netzwerkkommunikation (Nachfolger von SSL). Verhindert Mitlesen/Manipulation und authentifiziert den Server über Zertifikate. MQTT über TLS läuft typischerweise auf Port 8883.

**HTTP** — *Hypertext Transfer Protocol*, zustandsloses Anwendungsprotokoll nach dem Anfrage-Antwort-Prinzip (Client fragt, Server antwortet). Grundlage für Webseiten und REST-APIs; verschlüsselt als HTTPS.

**OPC-UA** — *Open Platform Communications Unified Architecture*, industrieller Kommunikationsstandard mit eingebauter Sicherheit und komplexem Datenmodell (Typen, Hierarchien). Wird oft zwischen SPS und SCADA eingesetzt; im Kurs nicht verwendet (wir nutzen MQTT).

**URL** — *Uniform Resource Locator*, eindeutige Adresse einer Ressource im Netzwerk, aufgebaut aus Schema, Host, optionalem Port, Pfad und ggf. Query/Fragment (z.B. `https://example.com:8080/pfad?param=wert`).

---

## MQTT

**MQTT** — *Message Queuing Telemetry Transport*, leichtgewichtiges Publish/Subscribe-Protokoll für Maschine-zu-Maschine-Kommunikation. IIoT-Standard für geringe Bandbreite; läuft über TCP, Standardport 1883 (8883 mit TLS).

**Broker** — Zentrale Vermittlungsinstanz im MQTT-Netzwerk: empfängt alle Nachrichten und leitet sie an die passenden Subscriber weiter. Entkoppelt Publisher und Subscriber — beide kennen nur den Broker. Kurs-Broker: `158.180.44.197:1883`.

**Client** — Jedes Gerät oder Programm, das sich mit dem Broker verbindet (identifiziert durch eine eindeutige Client-ID). Ein Client kann gleichzeitig Publisher und Subscriber sein.

**Publisher** — Client, der Nachrichten an ein Topic sendet (veröffentlicht). Er kennt die Subscriber nicht — die Verteilung übernimmt der Broker.

**Subscriber** — Client, der beim Broker Topics abonniert und alle dort eingehenden Nachrichten empfängt. Wildcards: `+` ersetzt genau eine Topic-Ebene, `#` alles ab dieser Stelle (nur am Ende).

**Topic** — Hierarchischer Kanal für Nachrichten, durch `/` getrennt (ähnlich einem Dateipfad), z.B. `aut/SoSe26/gruppe1/Fuellstand_rot`. Groß-/Kleinschreibung wird unterschieden.

**Payload** — Nutzlast einer MQTT-Nachricht (der eigentliche Inhalt). Kann eine beliebige Bitfolge sein: Text, JSON oder Binärdaten. MQTT gibt kein Format vor — Publisher und Subscriber müssen sich einigen.

**Quality of Service (QoS)** — Zustellungsgarantie einer Nachricht: **0** *AtMostOnce* (höchstens einmal, kann verloren gehen), **1** *AtLeastOnce* (mindestens einmal), **2** *ExactlyOnce* (genau einmal, höchster Overhead).

**Retain Flag** — Ist `Retain = TRUE`, speichert der Broker die letzte Nachricht eines Topics und liefert sie jedem neuen Subscriber sofort nach dem Abonnieren. Gespeichert wird nur der **letzte Wert**, keine Zeitreihe.

---

## REST & Web

**REST** — *Representational State Transfer*, Architekturstil für verteilte Systeme auf Basis von HTTP: zustandslos, ressourcenorientiert (alles über URLs adressiert), einheitliche Schnittstelle über HTTP-Methoden. Direkte Verbindung, kein Broker.

**API** — *Application Programming Interface*, definierte Schnittstelle, über die Programme miteinander kommunizieren. Häufigste Form im Web ist die REST-API; Antworten meist als JSON.

**Client-Server-Architektur** — Kommunikationsmuster, bei dem ein Client Anfragen stellt und ein Server diese direkt beantwortet — im Gegensatz zu Publish/Subscribe (MQTT) gibt es keinen Vermittler.

**HTTP-Methoden** — Definieren die Operation auf einer Ressource: **GET** (abrufen, ohne Nebeneffekt), **POST** (neu erstellen), **PUT** (ersetzen/aktualisieren), **DELETE** (löschen).

**Pull vs. Push** — *Pull*: der Empfänger fragt aktiv an, wenn er Daten braucht (REST/HTTP). *Push*: der Sender schiebt Daten, sobald sie anfallen (MQTT).

---

## Datenspeicherung, Zeitreihen & Visualisierung

**Datenbank** — Softwaresystem zur strukturierten, dauerhaften Aufbewahrung von Daten mit definierten Abfrage- und Änderungsmöglichkeiten.

**Data Warehouse** — Sammlung aufbereiteter Daten in fester Struktur, optimiert für Analysen und Berichte.

**Data Lake** — Sammlung von Rohdaten in beliebigem (auch unstrukturiertem) Format; die Struktur wird erst beim Lesen festgelegt.

**Hot / Warm / Cold Storage** — Speicher-Stufen nach Zugriffsgeschwindigkeit und Aktualität: *Hot* = Arbeitsspeicher/Echtzeit, *Warm* = persistente DB für Dashboards/Analysen, *Cold* = Langzeitarchiv.

**Zeitreihen-Datenbank (TSDB)** — Datenbank, optimiert für zeit-indizierte Messdaten: schnelles Anhängen (Append) und eingebaute Zeitfenster-Aggregation. Beispiele: InfluxDB, TimescaleDB.

**InfluxDB** — verbreitete Open-Source-Zeitreihen-Datenbank. Datenmodell aus *measurement/tag/field/timestamp*, Abfragesprache *Flux* (ab v2). Im Kurs als Industrie-Ausblick (Bonus).

**Measurement / Tag / Field / Timestamp** — das InfluxDB-Datenmodell: *measurement* = Art der Messung (wie eine Tabelle), *tags* = indizierte String-Metadaten zum Filtern, *fields* = die eigentlichen Messwerte (nicht indiziert), *timestamp* = Zeitpunkt.

**Kardinalität** — Anzahl eindeutiger Serien (Kombinationen aus measurement + Tags). Hoch-kardinale Werte (z.B. IDs) gehören in *Felder*, nicht in *Tags* — sonst wächst der Speicherbedarf unbegrenzt.

**Flux** — funktionale Abfragesprache von InfluxDB 2.x; Daten werden als Pipeline (`|>`) durch Filter- und Aggregationsschritte geschoben.

**Grafana** — De-facto-Standard für Zeitreihen-Dashboards: fragt Datenquellen (z.B. InfluxDB) ab und visualisiert sie live; Dashboards lassen sich als Code versionieren.

**Dashboard** — grafische Live-Übersicht ausgewählter Kennzahlen und Zeitreihen.

**Telegraf** — konfigurierbarer Daten-Collector ohne eigenen Code; sammelt über Plugins (MQTT, OPC-UA, Modbus …) und schreibt z.B. nach InfluxDB.

**TIG-Stack** — gängige IIoT-Pipeline aus **T**elegraf + **I**nfluxDB + **G**rafana (sammeln → speichern → visualisieren).

**Edge Computing / Edge Gateway** — Datenverarbeitung nah an der Maschine: ein lokales Gerät (Edge-Server / Industrie-PC) sammelt, speichert und visualisiert vor Ort, statt alle Rohdaten in die Cloud zu schicken.

**Unified Namespace (UNS)** — Architektur, in der ein zentraler Broker die „Single Source of Truth" ist und alle Daten in einem semantisch geordneten Topic-Baum (Enterprise → Site → Area → Line → Cell, vgl. ISA-95) liegen.

**Sparkplug B** — Spezifikation über MQTT mit standardisiertem Topic-Namespace, kompakten Protobuf-Payloads, Geburts-/Todes-Zertifikaten der Geräte und Store-and-Forward; für große, herstellerübergreifende Anlagen.

**Container / Docker** — gekapselte, reproduzierbare Laufzeitumgebung, die eine Anwendung mit allen Abhängigkeiten bündelt und isoliert ausführt.

**Docker Compose** — Werkzeug, das mehrere Container über eine YAML-Datei gemeinsam definiert und startet (z.B. InfluxDB + Grafana + Writer als ein Stack).

**Retention / Downsampling** — *Retention Policy*: alte Daten automatisch nach Ablauf löschen; *Downsampling*: Rohdaten zu gröberen Werten verdichten (z.B. Minuten- statt Sekundenmittel) für die Langzeit-Historie.

**SQLite** — serverlose, dateibasierte relationale Datenbank; in der Python-Standardbibliothek enthalten (`import sqlite3`).

---

## Kennzahlen & Dashboards (Industrie)

**OEE (Overall Equipment Effectiveness)** — zentrale Fertigungskennzahl: *Verfügbarkeit × Leistung × Qualität*. Im Kurs-Dashboard kommen Qualität (i.O./n.i.O.) und Leistung (Durchsatz) aus den vorhandenen Signalen; Verfügbarkeit bräuchte Stillstandszeiten, die der Simulator nicht liefert.

**i.O. / n.i.O.** — *in Ordnung / nicht in Ordnung*: Shop-Floor-Begriffe für Gutteil bzw. Ausschuss. Im Dashboard aus `is_cracked` abgeleitet (0 = i.O., 1 = n.i.O.).

**SPC (Statistical Process Control) / Checkweigher** — Prozessüberwachung: ein Messwert (z.B. Endgewicht) wird mit Mittellinie und Kontrollgrenzen (UCL/LCL = Mittel ± 3σ) dargestellt; Punkte außerhalb deuten auf einen Prozessfehler. Ein *Checkweigher* (Kontrollwaage) ist die klassische QS-Station einer Abfülllinie.

**Condition Monitoring** — Zustandsüberwachung von Maschinen über Sensorik (z.B. Vibration) mit Alarmschwellen; Grundlage für Predictive Maintenance.

**Durchsatz** — produzierte Einheiten pro Zeit (z.B. Flaschen/min) — die Leistungs-Komponente der Produktion.

**Ausschussquote / First Pass Yield** — Anteil der n.i.O.-Teile an der Gesamtmenge bzw. der Gutanteil — die Qualitäts-Komponente.

**Rückverfolgbarkeit (Traceability)** — jede Flasche ist über ihre eindeutige `bottle`-ID über alle Messungen (Dispenser, Gewicht, Qualität, Rezept) verknüpfbar; genau das ist die Basis des Joins in Aufgabe 12.1.2.

**Anlagenstatus / Heartbeat** — „läuft/steht"-Anzeige aus dem aktuellen Datenfluss: kommen in den letzten ~60 Sekunden Daten an, gilt die Anlage als laufend. Einfacher Liveness-Indikator ohne zusätzlichen Sensor.


