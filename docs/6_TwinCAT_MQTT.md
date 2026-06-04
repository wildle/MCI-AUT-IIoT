# TwinCAT MQTT-Client

## 🏆 Aufgabe 12.1.1 (20%)

Erweitert euer TwinCAT-Programm um einen MQTT-Client, der Sensordaten der Learning Factory an den Broker sendet:

- Füllstände aller Dispenser alle **10 Sekunden** übertragen
- Falls der Füllstand noch nicht implementiert ist: beliebigen anderen Wert senden
- **Alle Werte müssen retained gesendet werden**

### Topic-Schema

| Topic | Inhalt | Wann |
|-------|--------|------|
| `aut/SoSe26/<Gruppe>/$groupsname` | Name der Gruppe | einmalig beim Start |
| `aut/SoSe26/<Gruppe>/names` | Nachnamen der Mitglieder | einmalig beim Start |
| `aut/SoSe26/<Gruppe>/<Größe>` | Messwert (INT oder REAL) | alle 10s |
| `aut/SoSe26/<Gruppe>/<Größe>/$unit` | SI-Einheit als String | einmalig beim Start |

**Abgabe gilt als bestanden:** Werte kommen am Broker an und werden durch Retain gespeichert — überprüfbar mit [MQTT-Explorer](https://mqtt-explorer.com/).

---

## Schritt 1: IoT-Library einbinden

### 1a) IoT-Paket installieren (TwinCAT Package Manager)

Die MQTT-Funktionalität steckt im Function-Paket **TF6701 (TwinCAT 3 IoT Communication)**. Dieses Paket muss **einmalig pro Rechner** über den **TwinCAT Package Manager** installiert werden — sonst fehlt die Library `Tc3_IotBase` im Library Manager und der Funktionsbaustein lässt sich nicht kompilieren.

1. **TwinCAT Package Manager** öffnen (Windows-Start → „TwinCAT Package Manager")
2. Unter **Packages / Browse** nach `TF6701` bzw. `IoT` suchen (Paket „TwinCAT 3 IoT Communication")
3. Paket **installieren** und den Anweisungen folgen
4. Anschließend **Visual Studio / TwinCAT XAE neu starten**, damit die Library erkannt wird

> ⚠️ **Häufigste Stolperfalle:** Ohne diesen Schritt erscheint `Tc3_IotBase` nicht in der Library-Liste und der Build schlägt mit „library not found" fehl. Erst installieren, dann referenzieren.

### 1b) Library im Projekt referenzieren

Ist das Paket installiert, wird die Library `Tc3_IotBase` im **Library Manager** des Projekts referenziert (Rechtsklick auf **References → Add library → `Tc3_IotBase`**):

![](images/IoTBaseHinzufuegen.PNG)

---

## Schritt 2: SPS als Client konfigurieren

### Variablen deklarieren

```pascal
VAR
    fbMqttClient          : FB_IotMqttClient;
    sMessageToPublish     : STRING(255);
    tmrSendMessageInterval: TON := (PT := T#10S);
    first_cycle           : BOOL := TRUE;
    bStaticPublished      : BOOL := FALSE;
END_VAR
```

### Verbindung aufbauen (erster Zyklus)

```pascal
IF first_cycle THEN
    fbMqttClient.sHostName     := '158.180.44.197';
    fbMqttClient.nHostPort     := 1883;
    fbMqttClient.sTopicPrefix  := 'aut/SoSe26/<Gruppe>/';
    fbMqttClient.sClientId     := 'Publishing PLC';
    fbMqttClient.sUserName     := 'bobm';
    fbMqttClient.sUserPassword := 'letmein';
    first_cycle := FALSE;
END_IF

fbMqttClient.Execute(bConnect := TRUE);
```

### Startwerte publizieren (Retain = TRUE, einmalig)

```pascal
// Nur einmal nach dem Verbinden senden — bStaticPublished-Flag verhindert Wiederholung
IF NOT bStaticPublished THEN
    sMessageToPublish := '<GRUPPENNAME>';
    fbMqttClient.Publish(sTopic := '$$groupsname',
                         pPayload := ADR(sMessageToPublish),
                         nPayloadSize := LEN(sMessageToPublish) + 1,
                         eQoS := TcIotMqttQos.AtMostOnceDelivery,
                         bRetain := TRUE,
                         bQueue := TRUE);

    sMessageToPublish := 'g';
    fbMqttClient.Publish(sTopic := 'Fuellstand_rot/$$unit',
                         pPayload := ADR(sMessageToPublish),
                         nPayloadSize := LEN(sMessageToPublish) + 1,
                         eQoS := TcIotMqttQos.AtMostOnceDelivery,
                         bRetain := TRUE,
                         bQueue := TRUE);
    bStaticPublished := TRUE;
END_IF
```

### Messwerte periodisch publizieren (alle 10s)

```pascal
IF fbMqttClient.bConnected THEN
    tmrSendMessageInterval(IN := TRUE);
    IF tmrSendMessageInterval.Q THEN
        tmrSendMessageInterval(IN := FALSE);

        sMessageToPublish := UINT_TO_STRING(iIn_USS1);
        fbMqttClient.Publish(sTopic := 'Fuellstand_rot', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := FALSE);

        sMessageToPublish := UINT_TO_STRING(iIn_USS2);
        fbMqttClient.Publish(sTopic := 'Fuellstand_blau', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := FALSE);

        sMessageToPublish := UINT_TO_STRING(iIn_USS3);
        fbMqttClient.Publish(sTopic := 'Fuellstand_gruen', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := FALSE);
    END_IF
END_IF
```

---

## Hinweise zur Implementierung

- Legt einen oder mehrere neue Funktionsbausteine an (z.B. `FB_IotMqtt`)
- Mögliche Input-Variablen: `topic`, `payload`
- Überlegt, wie ihr **einmalige Nachrichten** (Gruppenname, Einheiten) von **periodischen Nachrichten** (Messwerte) trennt
- Der MQTT-Client hat ein Attribut `sTopicPrefix` für den ersten Teil des Topics — `Publish()` erhält nur den hinteren Teil als `sTopic`

---

## Bekannte Stolperfallen

**`$unit` in Structured Text:** Das `$`-Zeichen ist in TwinCAT eine Escape-Sequenz. Je nach Version muss es escaped werden:
```pascal
// statt '$unit' ggf.:
'$$unit'
```

**TwinCAT-Neustart beim ersten MQTT-Aktivieren:** Manchmal startet TwinCAT beim Aktivieren der MQTT-Verbindung neu. Strategie: zuerst Verbindung ohne Publish testen, dann schrittweise Nachrichten hinzufügen.

**Retain bei allen Werten:** Setzt Retain = TRUE für alle Topics — dann sehen auch Subscriber, die sich erst nach dem Start verbinden, sofort die aktuellen Werte.

---

## Komplette Implementierung

Fertiger Funktionsbaustein — `<GRUPPENNAME>` und `<Nachname1> <Nachname2>` ersetzen, dann in MAIN einbinden.

### Deklaration

```pascal
FUNCTION_BLOCK FB_iot
VAR_INPUT
    iUSS1 : UINT;
    iUSS2 : UINT;
    iUSS3 : UINT;
END_VAR
VAR
    fbMqttClient           : FB_IotMqttClient;
    sMessageToPublish      : STRING(255);
    tmrSendMessageInterval : TON := (PT := T#10S);
    first_cycle            : BOOL := TRUE;
    bStaticPublished       : BOOL := FALSE;
END_VAR
```

### Implementierung

```pascal
IF first_cycle THEN
    fbMqttClient.sHostName     := '158.180.44.197';
    fbMqttClient.nHostPort     := 1883;
    fbMqttClient.sTopicPrefix  := 'aut/SoSe26/<GRUPPENNAME>/';
    fbMqttClient.sClientId     := '<GRUPPENNAME>-LF';
    fbMqttClient.sUserName     := 'bobm';
    fbMqttClient.sUserPassword := 'letmein';
    first_cycle := FALSE;
END_IF

fbMqttClient.Execute(bConnect := TRUE);

IF fbMqttClient.bConnected THEN
    IF NOT bStaticPublished THEN
        sMessageToPublish := '<GRUPPENNAME>';
        fbMqttClient.Publish(sTopic := '$$groupsname', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := TRUE);
        sMessageToPublish := '<Nachname1> <Nachname2>';
        fbMqttClient.Publish(sTopic := 'names', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := TRUE);
        sMessageToPublish := 'g';
        fbMqttClient.Publish(sTopic := 'Fuellstand_rot/$$unit', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := TRUE);
        fbMqttClient.Publish(sTopic := 'Fuellstand_blau/$$unit', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := TRUE);
        fbMqttClient.Publish(sTopic := 'Fuellstand_gruen/$$unit', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := TRUE);
        bStaticPublished := TRUE;
    END_IF

    tmrSendMessageInterval(IN := TRUE);
    IF tmrSendMessageInterval.Q THEN
        tmrSendMessageInterval(IN := FALSE);
        sMessageToPublish := UINT_TO_STRING(iUSS1);
        fbMqttClient.Publish(sTopic := 'Fuellstand_rot', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := FALSE);
        sMessageToPublish := UINT_TO_STRING(iUSS2);
        fbMqttClient.Publish(sTopic := 'Fuellstand_blau', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := FALSE);
        sMessageToPublish := UINT_TO_STRING(iUSS3);
        fbMqttClient.Publish(sTopic := 'Fuellstand_gruen', pPayload := ADR(sMessageToPublish), nPayloadSize := LEN(sMessageToPublish) + 1, eQoS := TcIotMqttQos.AtMostOnceDelivery, bRetain := TRUE, bQueue := FALSE);
    END_IF
END_IF
```

### MAIN-Integration

```pascal
VAR
    fbiot : FB_iot;
END_VAR

fbiot(iUSS1 := iIn_USS1, iUSS2 := iIn_USS2, iUSS3 := iIn_USS3);
```

---

## Woher kommen die Füllstände?

Der Funktionsbaustein erwartet drei Eingänge `iUSS1`, `iUSS2`, `iUSS3` — die **Füllstände der drei Dispenser** (rot/blau/grün). Gemessen werden sie von den **Ultraschallsensoren** der Learning Factory.

**Gute Nachricht:** Diese Sensorwerte gibt es in eurem Projekt **bereits** — ihr habt sie in den vorherigen Einheiten (Messsteuerkette / Analog-Eingänge) schon eingelesen. Eine an einen physischen Eingang gekoppelte Variable erkennt ihr am `AT %I*`-Zusatz in der Deklaration, z.B.:

```pascal
// Beispiel aus der Analog-Input-Einheit — eine Eingangsvariable, die im I/O-Baum
// mit einer Hardware-Klemme verknüpft ist:
nPoti AT %I* : UINT;
```

Ihr müsst also **nichts neu verdrahten** — sucht die Variablen, die bei euch die drei Füllstände halten, und übergebt sie beim Aufruf an den FB:

```pascal
// in MAIN — eure eigenen, bereits vorhandenen Füllstand-Variablen einsetzen:
fbiot(iUSS1 := iIn_USS1,   // Füllstand rot  (Ultraschallsensor 1)
      iUSS2 := iIn_USS2,   // Füllstand blau (Ultraschallsensor 2)
      iUSS3 := iIn_USS3);  // Füllstand grün (Ultraschallsensor 3)
```

> 💡 Die Namen `iIn_USS1/2/3` sind nur Platzhalter — setzt die Variablennamen ein, die ihr in eurem Projekt für die Füllstände verwendet.

**Falls ein Füllstand bei euch noch nicht eingelesen ist:** Eingangsvariable deklarieren und im I/O-Baum mit der Sensor-Klemme verknüpfen (Rechtsklick auf die Variable → **Change Link** → passenden Kanal des Ultraschallsensors wählen):

```pascal
iIn_USS1 AT %I* : UINT;   // danach im I/O-Baum mit dem Ultraschallsensor-Kanal verlinken
```

> Notfalls genügt für die Abgabe **ein beliebiger Sensorwert** statt der Füllstände (siehe Aufgabenstellung oben) — Hauptsache, Werte kommen periodisch retained am Broker an.
