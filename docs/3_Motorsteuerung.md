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

# Motorsteuerung

**TwinCAT Motion Baukasten**
**Tc2_Mc2 Motion-Bibliothek**
**Beispiel Förderband**

![bg right:30%](images/Ziele_und_Partner_Planung_und_Umsetzung.jpeg)

--- 

## Grundlagen zur Motorsteuerung

![h:450](images/AchsenSchaubild.PNG)

---

### Ebenenansicht der Motorsteuerung

![bg right:60% h:600](images/EbenenMotorSteuerung.PNG)

---
## TwinCAT Motion Baukasten

![](images/MotionBaukasten.PNG)

[Video Link XPlanar](https://www.youtube.com/watch?v=IYw8fy9VQ24)


---

## Applikationsebene

Die Bewegungsarten der Tc2_Mc2 sind gemäß den Spezifikationen von PLCopen-motion-control gestaltet und umfassen drei Kategorien:

- Kontinuierliche Bewegung: Diese wird durch Geschwindigkeitseingaben definiert.
- Diskrete Bewegung: Diese wird durch Positions- oder Zeitangaben definiert.
- Synchronisierte Bewegung: Diese wird durch die Bewegung der Master-Achse definiert.

Die Unterschiede zwischen diesen Kategorien liegen in ihren erforderlichen Steuereingängen. Zusätzliche Eingaben wie maximale Geschwindigkeiten, Beschleunigungen und Bewegungsrichtung ergänzen die Definition der Bewegung.

---

## Kontinuierliche Bewegung 
**MC_MoveVelocity**

![bg right h:550](images/MC_MoveVelocity.PNG)


---

&emsp; &emsp; &emsp; &emsp; ![](images/Blockschaltbild_MCMoveVelocity.PNG)

---

## Diskrete Bewegung
**MC_MoveAbsolute**


![bg right h:550](images/MC_MoveAbsolute.PNG)

---

![bg h:400](images/Blockschaltbild_MC_MoveAbsolute.PNG)

---

## Diskrete Bewegung
**MC_MoveRelative**


![bg right h:550](images/MC_MoveRelative.PNG)

---

![bg h:350](images/Blockschaltbild_MC_MoveRelative.PNG)

---

## Achsen Referenz (Axis_Ref)

- Der Datentyp AXIS_REF enthält Information zu einer Achse. AXIS_REF ist eine Schnittstelle zwischen der SPS und der NC und wird den MC-Funktionsbausteinen als Referenz auf eine Achse mitgegeben.
- Dieser Eingang ist keine gewöhnliche Variable, sondern eine *In_Out*-Variable.
- Dadurch werden Daten des Datentyps *Axis_Ref* empfangen und ausgegeben.
- Dies ermöglicht eine bidirektionale Kommunikation mit der NC-Achse, wie in oben stehender Abbildung des Ebenenmodells zwischen SPS-Task und NC-Task dargestellt.
- Jede Achse in der Anlage erhält eine individuelle Achsenreferenz.
Dadurch kann jede Achse eindeutig durch den Aufruf eines Bewegungsbausteins angesteuert werden

---

## Zustandsdiagramm der Tc2_Mc2

![bg right:60% h:600](images/StateGraphTc2Mc2.PNG)

---

### Organisationsbausteine der Tc2_Mc2

**MC_Power**
**MC_Reset**

---

![bg h:400](images/Blockschaltbild_MC_Power.PNG)

---

![bg h:300](images/Blockschaltbild_MC_Reset.PNG)

---

## NC-Achsenebene


---

## Achsen Konfiguration

![bg right:70% h:500](images/AllgemeineNC_Konfiguration.PNG)

---

### Parameter der Achse

 - Bezugsgeschwindigkeit
 - Max. Geschwindigkeit, Beschleunigung, Verzögerung
 - Normal Beschleunigung, Verzögerung, Ruck
 - Manuele Beweungsparameter und Homing (Referenzfahrten)
 - Endlagenüberwachung Software
 - Überwachungsparameter (Schleppabstand, Positionsbereichsüberwachung)
 - Weitereparameter wie Eilgang oder Sollwert Generator u.v.m. 

---

## Encoder Konfiguration


![](images/AllgemeineEnc_Konfiguration.PNG)


---

### Parameter des Encoders

 - Geberrichtung
 - Skalierfaktoren und Modulofaktor
 - Filter
 - Referenzfahrtparameter (Endschalter, MC_Home, usw.)
 - Encodermode (Pos, PosVel, PosVelAcc)

---
## Reglerimplementierung

![h:400](images/AllgemeineCtrl_Konfiguration.PNG)

---

### Reglerparameter

- Reglertyp 
- Reglerspezifische Paramter

---

### Online-Betrieb (Jogging Mode)

SPS-Pausiert oder Achsenreferenz nicht vergeben!

![bg right:70% h:450](images/Online_Betrieb.PNG)

---
## Hardware Interface Ebene

![bg right:60% h:500](images/Hardwarekomponenten_Motorsteuerung.png)

---

### Hardware Interface Parameter (CoE-Parameter)

- Wiklungswiderstände
- Wiklungsinduktivitäten
- Fullsteps (Schrittmotor)
- Nennspannung und -strom
- Tunningbefehle (automatisierte Systemidentifikation)
- Encoderparameter
- u.v.m. 

---

## Hardware (Motoren und Encoder)

![bg right 60% h:550](images/Achsenhardware.PNG)

---

### Inkremetalencoder

![bg right:60% h:500](images/InkrementalEncoder.PNG)


---

### Absolute Encoder
![bg right:70% h:200](images/BinärGrayCode.PNG)

---

![bg h:450](images/AbsoluteEncoderAufbau.PNG)

---

### Multiturn Encoder (Hall Effekt)

![h:400](images/AbsoluteEncoder.PNG)

https://www.youtube.com/watch?v=wpAA3qeOYiI
https://www.youtube.com/watch?v=jkXsqwXNVlw



