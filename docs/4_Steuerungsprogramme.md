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
<!-- header: Entwicklung von Steuerungsprogrammen -->

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

Pseudobeispiel in TwinCAT...

![bg right:65% h:600](images/GenerischeGraphengruppe.PNG)

---

## Graphengruppen

Drei Grundgedanken sollten dabei Hilfe bieten, um eine Graphengruppe zu erstellen.
- Globale Initialisierung
- Sequentielle Startsignale
- Signalaustausch der Funktionseinheiten

---
### Globale Initialisierung

![bg right:70% h:500](images/GlobaleInit.PNG)

---
### Sequentielle Startsignale

![h:450](images/Ablaufdiagramm_Gesamtprozess.PNG)

---
### Signalaustausch der Funktionseinheiten

![h:450](images/SignalAustausch.PNG)

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

### Abfüllmodul 

![](images/Schema_DispenserEinheit.PNG)


![bg right h:400](images/Schema_Abfüllmodul.PNG)

---

### Manipulatoreinheit 

![](images/Programmstruktur_Manipulatoreinheit.PNG)

---

#### Ablaufdiagramm Robotermodul

![bg right:70% h:500](images/Ablaufdiagramm_Robotermodul.PNG)