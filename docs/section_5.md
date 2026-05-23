---
marp: true
author: Serafin Kollegger & Julian Huber
size: 16:9
footer: Automatisierungstechnik
class: inver
header: 
theme: lemon

---

<!-- paginate: true -->

**SoSe 2024**
Serafin Kollegger & Julian Huber

# Automatisierungstechnik
**Steuerungsprogramme**
**Hierarchischer Ansatz von Graphengruppen**

---


# Entwicklung von Steuerungsprogrammen 

- Funktionseinheiten können definiert werden, um die Komplexität von Zustandsgraphen zu reduzieren.
- Sie repräsentieren das Verhalten einzelner Komponenten.
- Übergeordnete Funktionsgruppen steuern diese Einheiten.
- Eine sorgfältige Auswahl der Komponenten ist entscheidend.
- Eine hierarchische Struktur kann für eine zweckmäßige Verknüpfung der Funktionseinheiten genutzt werden.

---

![h:600](images/AblaufdiagrammErsterTeil.PNG)

![bg right h:550](images/AblaufdiagrammZweiterTeil.PNG)

---

## Hierarchischer Ansatz

![bg right:65% h:550](images/AnlagenEbenenmodel.PNG)

---

## Hierarchischer Ansatz

- Definition von drei Hauptebenen für den hierarchischen Ansatz
- Oberste Ebene: Maschinen-/Anlagen-Ebene (Machine Level)
- Darunter: Modul-Ebene (Modul Level)
- Unterste Ebene: Prozess-Ebene (Process Level)
- Funktionseinheiten werden auf der Prozess-Ebene gesteuert
- Anpassung der Programmstruktur an den mechanischen Aufbau

---

### Maschinen Ebene

- Zusammenfassung von Funktionen auf der Maschinen-/Anlagen-Ebene
- Enthält Zustandsgraphen für Betriebsmodus und Betriebsfreigaben
- Zustandsgraphen, die das Verhalten der Steuerung bei Störungen festlegen, sind ebenfalls enthalten
- Funktionen auf dieser Ebene sind von übergeordneter Bedeutung

---

### Modul Ebene

- Implementierung von Zustandsgraphen auf der Modul-Ebene
- Hauptaufgabe: Koordination zwischen verschiedenen Funktionseinheiten
- Zusammenführung der Zustandsgraphen der Funktionseinheiten wird als Graphengruppe bezeichnet
---

### Prozessebene 

- In der Prozess Ebene werden die Funktionseinheiten durch Zustandsgraphen beschrieben
---

### Erweiterungen des Ebenenansatzes

**Submodule**

- Einführung zusätzlicher Submodul-Ebenen für komplexe Anlagen
- Die Modul-Ebene besteht aus Submodulen anstelle von Funktionseinheiten
- Submodule sind wiederum aus Funktionseinheiten zusammengefasst

**Hardware Ebene**
- Ziel: Herstellung eines hardwareunabhängigen Steuerungscodes
- Steuersignale in der Prozessebene werden allgemein und abstrahiert gehalten
- Die tatsächlich angeschlossene Hardware wird durch eigene Funktionsbausteine gesteuert
- Dadurch kann ein Großteil des Programmieraufwands bereits erledigt werden, bevor die Anlagenentwicklung abgeschlossen ist
- Flexibilität, da sich Sensoren und Aktoren noch ändern können, ohne den Programmieraufwand zu erhöhen

---

## Graphengruppen

![bg right:65% h:600](images/GenerischeGraphengruppe.PNG)

---

## Graphengruppen

Drei Grundgedanken sollten dabei Hilfe bieten, um eine Graphengruppe zu erstellen.
- Globale Initialisierung
- Sequentielle Startsignale
- Signalaustausch der Funktionseinheiten

---
### Globale Initialisierung


- Globale Initialisierung bedeutet, dass mit dem Signal der Betriebsfreigabe zur Produktion (bEnable) alle in diesem Modul verwendeten Funktionseinheiten eine Betriebsfreigabe erhalten.
- Alle Initialisierungsprozesse sind innerhalb der Funktionseinheiten programmiert und werden nur von der Graphengruppe ausgelöst.
- Ebenso ist die Graphengruppe im Betriebszustand State.Ready, sobald alle enthaltenen Funktionseinheiten in diesem Zustand sind.

---
### Sequentielle Startsignale

- Sequenzielle Startsignale bedeuten, dass nicht alle Funktionseinheiten mit dem Startsignal bExecute gestartet werden können.
- Stattdessen soll der Prozessablauf gemäß dem Ablaufdiagramm der gesamten Anlage durchgeführt werden.
- Dafür ist ein Zustandsgraph für die Ablaufbeschreibung zu erstellen, in dem die Koordination der Funktionseinheiten übernommen wird.

---
### Signalaustausch der Funktionseinheiten

- Um den Ablauf ordnungsgemäß umzusetzen, ist ein Signalaustausch zwischen den Funktionseinheiten erforderlich.
- Idealerweise basiert diese Informationsweitergabe für alle Funktionseinheiten auf dem gleichen Prinzip.
- Das bedeutet, dass eine Funktionseinheit, die im Ablauf vorher steht und ihre Arbeitsschritte abgeschlossen hat (im Betriebszustand .eState = State.Done), die nachfolgende Funktionseinheit starten lässt.
- Gleichzeitig kann die vorherige Funktionseinheit durch NOT .bExecute zurückgesetzt werden.
- Weitere Eingangssignale für die Funktionseinheiten können ebenfalls in der Graphengruppe gesteuert werden, wie beispielsweise Zähler, Timer, Schwellwerte usw.

---

## Programmstruktur abhängig von Funktionseinheiten

**Variante 1**
![](images/V1Abfüllmodul.PNG)

---

**Variante 2**
![](images/V2Abfüllmodul.PNG)

---

**Variante 3**
![](images/V3Abfüllmodul.PNG)

---
## Basisaufgaben 

---

### Beschreibung Transporteinheit

![](images/SchemaTransporteinheit.PNG)

---

![](images/AblaufdiagrammTransporteinheit.PNG)


![bg right h:300](images/Blockschaltbild_Transporteinheit.PNG)

---

### Abfüllmodul - 20 Punkte

![](images/Schema_DispenserEinheit.PNG)


![bg right h:400](images/Schema_Abfüllmodul.PNG)

---

### Manipulatoreinheit - 20 Punkte

![](images/Programmstruktur_Manipulatoreinheit.PNG)

---

#### Ablaufdiagramm Robotermodul

![bg right:70% h:500](images/Ablaufdiagramm_Robotermodul.PNG)