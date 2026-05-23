---
marp: true
author: Serafin Kollegger, Julian Huber & Lenard Wild
size: 16:9
footer: Automatisierungstechnik
class: invert
header: 
theme: lemon 


# Strg+[ ] for Options 

---

<!-- paginate: true -->

# Datenverarbeitung in der industriellen Produktion


Serafin Kollegger, Julian Huber & Lenard Wild

![bg right:30%](images/Ziele_und_Partner_Planung_und_Umsetzung.jpeg)


---

<!-- _class: white -->

 

![](images/1573184945568.png)

  


---

## Projekt

<!-- _class : white -->

 

![h:400](images/Architecture.svg)

  

---

## Leistungsbewertung

- Das Projekt wird in den bestehenden Gruppen bearbeitet
- Der Teil Datenverarbeitung (Lenard Wild) wird mit 1/4 gewichtet, der allgemeine Teil (Serafin Kollegger) mit 3/4 gewichtet
- *Basisaufgaben für 100%*
  - 🏆 Aufgabe 12.1 (20% Punkte): MQTT-Client für SPS
  - 🏆 Aufgabe 12.2 (40% Punkte): Datenspeicherung und Visualisierung
  - 🏆 Aufgabe 12.3 (20% Punkte): Regressionsmodell für Endgewicht
  - 🏆 Aufgabe 12.4 (20% Punkte): Klassifikationsmodell für defekte Flaschen
- Abgabe als git-Repository mit allen Quellcodes und Dokumentation in Form einer Markdown-Datei
- Abgabe bis 03.07.2026


---

## Industrial Internet of Things

<!-- _class: white -->

 

![](images/csm_Indu-Sol_Fachpressetage_2017_Bild_2_b15566fced.jpg)

  


###### https://www.ien-dach.de/artikel/netzwerke-der-zukunft-ot-security-im-kontext-von-industrie-40/

---

### Klassische Automatisierungspyramide

* **Feldebene**: Sensoren und Aktoren, welche über analoge Ein- und Ausgänge an die Steuerung angeschlossen sind
* **Steuerungsebenen**: SPS, die die Steuerung einzelner Anlagen 
* **Prozessleitebene**: Supervisory Control and Data Acquisition (SCADA) Systeme, die die Steuerung mehrerer Anlagen übernehmen
* **Betriebsleitebene**: Manufacturing Execution Systems (MES), die den gesamten Produktionsprozess überwachen und steuern

---



<!-- _class: white -->

 

![](images/csm_Indu-Sol_Fachpressetage_2017_Bild_2_b15566fced.jpg)

  

* Datenmenge nimmt von unten nach oben ab
* nur die **relevantesten** Daten werden nach oben weitergegeben
* Zugriff und Steuerungsverantwortung klar definiert

###### https://www.ien-dach.de/artikel/netzwerke-der-zukunft-ot-security-im-kontext-von-industrie-40/


---

### Fragenstellungen

* **Intelligence in der Produktion**: Wie werden aus den Daten ein Mehrwert generiert?
* **Security und Savety**: Wie wird die Sicherheit und Verfügbarkeit der Systeme gewährleistet?
* **Architektur**: Wie werden die zusätzlichen Daten übertragen und gesammelt?


---


## Intelligence in der Produktion


### Effizienzsteigerung durch Automatisierung


 

<iframe width="840" height="472" src="https://www.youtube.com/embed/6LmJmnT4D3c?si=yP5kpU1BGGYhFE7i" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

  


---

### Steigerung der Verfügbarkeit

 

<iframe width="840" height="472" src="https://www.youtube.com/embed/5ChEy3lIqMQ?si=85b5VqTbfMhnOBeh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


  

---

### Flexibilisierung der Produktion

 

<iframe width="840" height="472" src="https://www.youtube.com/embed/IGlUyDVtT4A?si=XxxCs5NLrlED2IkF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


  

---

### Anwendungsfall



* Daten von außerhalb der Automatisierungstechnik müssen integriert werden (z.B. Bilderkennung)
* Daten, die bisher nur innerhalb der Automatisierungstechnik genutzt wurden, müssen auch außerhalb genutzt werden (z.B. Schwingungsdaten)
* Daten von verschiedenen Anlagen müssen zusammengeführt werden (z.B. Planungsdaten und Produktionsdaten)



![bg right h:300](images/5336265.jpg)


---



![](images/ThingSpeak-Server_c_MathWorks-Beckhoff.png)

---

### Open Systems Interconnection Model

![bg w:600 right](images/ISO-OSI-7-Schichten-Modell_(in_Deutsch).svg.png)

* Entscheidung für ein geeignetes Protokoll für die Vernetzung
* Auf den Netzwerkschichten
  * TCP/IP-Protokollfamilie
  * Standard für das Internet
  * Verfügbarkeit Hardware
  * Echtzeitfähigkeit nicht benötigt
  * Kabelgebundene und drahtlose Kommunikation

---

### Anwendungsbezogene Schichten (Topologie)


Auswählen einer Teilmenge (Kombinationen ohne Wiederholung)

>**Satz 1.4** Die Anzahl der Möglichkeiten, $k$ Gegenstände aus einer vorgegebenen Menge von $n$ Gegenständen auszuwählen (ohne Rücksicht auf die Reihenfolge), ist

$$\frac{n!}{k!(n-k)!} = \binom{n}{k}$$

Wie viele peer2peer-Verbindungen ($k=2$) gibt es bei $n$ Geräten?

$$\binom{n}{2} = \frac{n!}{2!(n-2)!} = \frac{n(n-1)}{2}$$


![bg right:20% h:300](images/P2P-network.svg)

---

### Anwendungsbezogene Schichten (Flexibilität vs. Standardisierung)

- Consumer-Produkte erfordern ein hohes Maß an Standardisierung (Plug&Play)
- Beispiel: [Datenmodell einer Leuchte nach Matter](https://csa-iot.org/all-solutions/matter/)

 

![](images/matter_pt2_02.png)

  

- Industrielle Anwendungen erfordern mehr **Flexibilität und Anpassungsfähigkeit**

---

### Folge

* Wir benötigen Protokolle, die eine **hohe Flexibilität** und **Anpassungsfähigkeit** bieten (z.B. bei der Datenmodelle)
* Diese sollen auf dem TCP/IP-Stack aufbauen

#### MQTT - Message Queuing Telemetry Transport

* Anwendungsschichtprotokoll kompatibel mit TCP/IP
* Sternförmige Kommunikation
* Publish-Subscribe-Modell



#### REST - Representational State Transfer

* Anwendungsschichtprotokoll kompatibel mit TCP/IP
* Peer-2-Peer Kommunikation
* Client-Server-Modell



---

## Message Broker für Publish-Subscribe-Modell


 

![](images/1_vDE8StT8wJQL46yL4LWMww.png)

  


###### https://medium.com/@jaydev.dave93/what-is-mqtt-protocol-c6a0cafffa8c

---

### MQTT - Message Queuing Telemetry Transport

* offenes Netzwerkprotokoll für Machine-to-Machine-Kommunikation 
* leichtgewichtig und häufig für-IoT Sensoren
* Daten werden in Form von Nachrichten übertragen (Beliebige Bitfolgen)
* Nachrichten werden in **Topics** veröffentlicht und abonniert
* Viele weitere [Alternativen](https://en.wikipedia.org/wiki/Message_broker): Apache Kafka, etc.

![bg right:23% w:300](images/mqtt-logo.png)

---

### MQTT-Broker

* Zentrale Instanz (Server), welche Nachrichten verteilt
  * IP-Adresse bzw. URL (Uniform Resource Locator)
  * Port
  * Sicherheitseinstellungen
    * Username und Passwort
    * Verschlüsselung mittels TLS
  * Bildet Message-Queues für Topics
* Mehrere Akteure können sich als Clients verbinden
  * Producer sind Clients, welche Nachrichten erzeugen
  * Consumer sind Clients, die Nachrichten abrufen

![bg right:33% w:400](images/1_vDE8StT8wJQL46yL4LWMww.png)


###### https://medium.com/@jaydev.dave93/what-is-mqtt-protocol-c6a0cafffa8c

---


### Hosting von MQTT-Brokern

* Web-Service
* Fremd-gehostet in der Could z.B. mit [hivemq](https://console.hivemq.cloud/)
* Selbst gehostet z.B. mit [Mosquitto](https://mosquitto.org/) z.B. auch lokal auf einer Soft-SPS, sofern kein Internetzugriff benötigt wird

![bg right w:600](images/MQTT_hive.png)

###### https://console.hivemq.cloud/

---

### Beispiel für Nachrichten

![](images/MQTT-Explorer.png)

* Beliebige Bitfolgen
  * z.B. UTF-codierte Texte im JSON-Format 
* `topic`: thematische Zuordnung (Liste der Empfänger)
  * Struktur ähnlich Dateisystem
  * Hierarchie-Ebenen mit `/` getrennt
* `payload`: Inhalt der Nachricht (Text einer Email)



---

#### Producer (z.B. eine Soft-SPS)

* Definieren Verbindung zu einem Broker/Server über URL
* veröffentlichen (publishen) Nachrichten zu einem oder mehren Topics
* **Push-Prinzip**: Producer legen selbst fest, wann sie eine Nachricht veröffentlichen

![bg right:33% w:400](images/1_vDE8StT8wJQL46yL4LWMww.png)

---

**Beispiel `paho-mqtt 2.1.0` in Python**

```python
import paho.mqtt.client as mqtt
broker = "158.180.44.197"
port = 1883
topic = "at/house/bulb1"
payload = "on"

# create function for callback
def on_publish(client, userdata, flags, reasonCode, properties):
    print("data published \n")

# create client object
mqttc = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2)
mqttc.username_pw_set("bobm", "letmein")              

# assign function to callback
mqttc.on_publish = on_publish                          

# establish connection
mqttc.connect(broker,port)                                 

# publish
return_code = mqttc.publish(topic, payload)

mqttc.disconnect()
```

---

#### Consumer

* Definieren Verbindung zu einem Broker/Server über URL
* Empfangen alle Nachrichten zu von ihnen definierten Topics
* Laufen i.d.R. ein einem endlosen Loop, um Nachrichten zu empfangen
* Grundsätzlich werden nur die Nachrichten empfangen, die während der Laufzeit des Programms gesendet werden (Ausnahme: Nachrichten mit Retain-Flag)

![bg right:33% w:400](images/1_vDE8StT8wJQL46yL4LWMww.png)

---

**Beispiel `paho-mqtt` in Python**

```python
import paho.mqtt.client as mqtt
broker = "158.180.44.197"
port = 1883
topic = "at/house/bulb1"
payload = "on"

# create function for callback
def on_message(client, userdata, message):
    print("message received:")
    print("message: ", message.payload.decode())
    print("\n")

# create client object
mqttc = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2)
mqttc.username_pw_set("bobm", "letmein")              

# assign function to callback
mqttc.on_message = on_message                          

# establish connection
mqttc.connect(broker,port)                                 

# subscribe
mqttc.subscribe(topic, qos=0)

# Blocking call that processes network traffic, dispatches callbacks and handles reconnecting.
#mqttc.loop_forever()

while True:
    mqttc.loop(0.5)
```




---

### MQTT-Wildcards

* Möchte man mehr als ein Topic erhalten
  ```
  building_1/smart_meter/messwerte
  building_2/smart_meter/messwerte
  building_1/raum_1/temperatur
  building_1/raum_2/temperatur
  building_2/raum_1/temperatur
  building_2/raum_2/temperatur
  ```
* Single Level: Temperatur in allen Räumen von Gebäude 1
  ```  building_1/+/temperatur  ```
* Multi Level: Alles zu Gebäude 1
  ```  building_1/#  ```




---

### Quality of Service (QoS)

* Gibt an, wie viel Wert auf die Zustellung der Nachrichten Wert gelegt wird
* **QoS 0:** *'Fire and Forget'*
* **QoS 1:** Mindestens ein zweites Senden nach Time-out (Duplikate möglich!) 
* **QoS 2:** Sicherste Möglichkeit, jedoch hoher Overhead

![bg right w:600](images/MQTT_QoS_mobile.jpg)

###### https://www.techtarget.com/iotagenda/definition/MQTT-MQ-Telemetry-Transport

---

### Retain Flag

* Einzelne Topics können mit Retain Flag versehen werden
* Der Broker speichert die letzte Nachricht des Topics
* zum Überschreiben: leere Message

 

![h:200](images/1_BGq8wqT3PeBng84v2csOkg.webp)

  

* Neue Subscriber bekommen mit, wann die letzte Nachricht kam 
(z.B. bei seltenen Ereignissen, wie Fensterkontakte)
* Beschreibung eines Topics soll dokumentiert werden (z.B. `building_1/smart_meter/messwerte/$unit`)

###### https://emqx.medium.com/mqtt-5-0-features-retain-message-104698b070d1

---

## 🔧🦾👩‍💻 Praxisbeispiel und Übung

<!-- _class : white -->

 

![h:400](images/Architecture.svg)

  


---


## 🏆 Aufgabe 12.1.1 (20% Punkte)


- Erweitern Sie das Programm um eine Funktionalität, die Daten des Ultraschallsensors aller Dispenser der Learning Factory an den im Beispiel angegeben MQTT-Broker sendet. Die Füllstände sollen alle 10 Sekunden übertragen werden (wenn Sie die Füllhöhen noch nicht implementiert haben, senden Sie einen beliebigen anderen Wert aus ihrem Code).
- Stellen Sie sicher, dass alle Werte retaiend gesendet wird.

---

- Erstellen Sie ein Topic, das den Namen Ihres Teams enthält und folgendem Schema folgt:
    - `aut/SoSe26/<Name der Gruppe>/$groupsname` : `<Name der Gruppe>` - Bitte `<>` mit eigenen Namen füllen. Wird nur einmal beim Systemstart gesendet
    - `aut/SoSe26/<Name der Gruppe>/names` : `<Name der Personen>` - String mit Nachnamen, wird nur einmal beim Systemstart gesendet
    - `aut/SoSe26/<Name der Gruppe>/<Name der übertragen Größe>` : `<INT oder REAL>`	
    - `aut/SoSe26/<Name der Gruppe>/<Name der übertragen Größe>/$unit` : `<STR mit SI-konformer Einheit>` - wird nur einmal gesendet
- Nutzen Sie z.B. die Software [MQTT-Explorer](https://mqtt-explorer.com/) um die Werte zu überprüfen.
- **Abgabeformalien:** Die Aufgabe gilt als abgeschlossen, wenn die Werte auf dem Broker ankommen und dort durch das Retain gespeichert werden.

---

### IoT-Library Einbinden

* Für diese Aufgabe muss die IoT-Library `TC3_IotBase` in der Beckhoff-SPS eingebunden werden

 

![h:400](images/IoTBaseHinzufuegen.PNG)

  

---

### SPS als Client Konfigurieren

```ST
VAR
    fbMqttClient: FB_IotMqttClient; // MQTT client with initialization
    sTopicToPublish: STRING(255) := 'Temperature'; // MQTT topic for temperature
    sMessageToPublish: STRING(255); // Message buffer
    tmrSendMessageInterval: TON := (PT := T#1S); // Interval timer for publishing
    ai_RoomTemperature: INT := 1; // Room temperature from thermocouple
    first_cycle : BOOL := TRUE; // Flag to check if it's the first cycle
END_VAR
```

---

```ST
IF first_cycle THEN
  fbMqttClient.sHostName := '158.180.44.197';
  fbMqttClient.nHostPort := 1883;
  fbMqttClient.sTopicPrefix := 'aut/SoSe26/<Gruppe>/';
  fbMqttClient.sClientId := 'Publishing PLC';
  fbMqttClient.sUserName := 'bobm';
  fbMqttClient.sUserPassword := 'letmein';
  first_cycle := FALSE; // Set flag to FALSE after first cycle

END_IF


fbMqttClient.Execute(bConnect := TRUE);

IF fbMqttClient.bConnected THEN
    tmrSendMessageInterval(IN := TRUE);
    IF tmrSendMessageInterval.Q THEN
        tmrSendMessageInterval(IN := FALSE);
        sMessageToPublish := REAL_TO_STRING(ai_RoomTemperature / 10.0);
        
        fbMqttClient.Publish(
            sTopic := sTopicToPublish,
            pPayload := ADR(sMessageToPublish),
            nPayloadSize := LEN(sMessageToPublish) + 1,
            eQoS := TcIotMqttQos.AtMostOnceDelivery,
            bRetain := FALSE,
            bQueue := FALSE
        );
    END_IF
END_IF
```

---

- legen Sie einen oder mehrere neue Funktionsbausteine an
- Mögliche Input-Variablen sind `topic`, `payload`
- Überlegen Sie, wie sie die einmaligen Nachrichten (z.B. Gruppenname) von den periodischen Nachrichten trennen
- der MQTT-Client hat ein Attribut `sTopicPrefix`, welches den ersten Teil des Topics definiert
- Zudem kann der `publish`-Funktion ein `sTopic` übergeben werden, welches den zweiten Teil des Topics definiert
- Beachten Sie, dass es bei Zeichenketten sogenannte Escape-Sequenzen gibt, die in der Dokumentation der Beckhoff-SPS zu finden sind. Die können z.B. bei der Definition von`"$unit"` Probleme machen


---

## REST - Representational State Transfer

### Client-Server-Architektur

<!-- _class: white -->

- **Client** sendet **Anfangen**
- **Server**: Bearbeitet Anfragen und sendet **Antworten**

![bg right width:600](images/ClientServer.svg)

---

### Hypertext Transfer Protocol für REST-Services

* **HTTP** ist ein Protokoll auf Anwendungsschicht
* Bekannt für die Übertragung von Webseiten
* Aber auch für die Übertragung aller anderen Daten geeignet (sofern Stateless)
* Unter den meisten Betriebssystemen verfügbar (`cURL`)

*Website über Power Shell*
```PS
curl "www.google.com"
```


*[Wetter-API](https://open-meteo.com/en/docs) über Power Shell*
```PS
curl "https://api.open-meteo.com/v1/forecast?latitude=47.26&longitude=11.40&current=temperature_2m"
```

*Allgemeine Synthax*

```PS
curl "<URL>/?<query_parameter>"
```

---


### Adressierung über Uniform Resource Locator (URL)

* Uniform Resource Locator
* identifiziert und lokalisiert eine Ressource, beispielsweise eine Webseite, über die zu verwendende Zugriffsmethode (zum Beispiel das verwendete Netzwerkprotokoll wie HTTP oder FTP)
* `host` kann auch eine IP-Adresse sein

`<scheme>:<scheme-specific-part>`

```
      |-------------------- Schema-spezifischer Teil ----------------------|
      |                                                                    |
https://maxmuster:geheim@www.example.com:8080/index.html?p1=A&p2=B#ressource
\___/   \_______/ \____/ \_____________/ \__/\_________/ \_______/ \_______/
  |         |       |           |         |       |          |         |
Schema Benutzer Kennwort      Host      Port    Pfad      Query    Fragment
```

[Quelle](https://de.wikipedia.org/wiki/Uniform_Resource_Locator#Schema-spezifischer_Teil_(scheme-specific_part))

---

### Antwort auf HTTP-Request

*[Wetter-API](https://open-meteo.com/en/docs) über Power Shell*
```PS
curl "https://api.open-meteo.com/v1/forecast?latitude=47.26&longitude=11.40&current=temperature_2m"
```


```
StatusCode        : 200
StatusDescription : OK
Content           : {"latitude":47.260002,"longitude":11.4,"generationtime_ms...
Forms             : {}
Headers           : {[Transfer-Encoding, chunked], [Connection, keep-alive], [Content-Type, application/json; charset=utf-8], [Date...
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 321
```

* Neben den Daten (`Content`) enthält die Antwort auch Metadaten (`Header`, `StatusCode`, ...)


---



### HTTP mit Python - Requests

* Zur Kommunikation mit REST-Services wird das `requests`-Modul verwendet
* Zum Anbieten eines REST-Services wird das `flask`-Modul verwendet

```Python
# Paket mit dem Python wie ein Browser agieren kann
import requests

# Request an Server mit MCI-Website
response = requests.get('https://www.mci.edu/de/')

# HTML Status
print(response.status_code) # 200 - heiß alles OK

print(response.headers['content-type']) # text/html; charset=utf-8 # Also ein HTML-Dokument

print(response.text) # <!doctype html> <html lang="de" class="no-js"> 	<head> ...
```

---

### HTTP mit Python - Server

```Python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'
```

```PS
$env:FLASK_APP = "hello.py"
python -m flask run
 * Running on http://127.0.0.1:5000/
```


---

### HTTP-Methoden

#### GET

* Fordert die angegebene Ressource vom Server an 
* GET weist keine Nebeneffekte auf. Der Zustand am Server wird nicht verändert, weshalb GET als sicher bezeichnet wird.
* Rückgabe kann eine JSON sein

```Python
# Paket mit dem Python wie ein Browser agieren kann
import requests

# Request an Server mit MCI-Website
response = requests.get(url = 'http://api.citybik.es/v2/networks/stadtrad-innsbruck')
# > {"network":{"company":["Nextbike GmbH"],"href":"/v2/networks/stadtrad-innsbruck","id":"stadtrad-innsbruck"," <...>
```

---

#### POST

* Fügt eine neue (Sub-)Ressource unterhalb der angegebenen Ressource ein. Da die neue Ressource noch keinen URI besitzt, adressiert der URI die übergeordnete Ressource. Als Ergebnis wird der neue Ressourcenlink dem Client zurückgegeben. 

```Python
my_data = """{
  "contact": {
      "id": "1",
      "firstName": "Julian",
      "lastName": "Huber"
    }
}"""

# Übermittlung und speichern der response Objects
response = requests.post(url = 'https://3d3m9.mocklab.io/v1/contacts', data = my_data, headers = {'Content-Type': 'application/json'})

response.text
# > Created a new contact!
```

---

```Python
@app.route('/number_of_bottles', methods=['GET', 'POST'])
def number_of_bottles():
    if request.method == 'POST':
        return f"Number of bottles changed to {request.data.decode('utf-8')}", 200
    elif request.method == 'GET':
        return "42", 200
    else:
        return "Method not allowed", 405
```


---

### Stuktur einer HTTP-Anfrage

- **Request-Line**: Methode, URI und Protokollversion
- **Header**: Metadaten zur Anfrage, z.B. Authentifizierung
- **Leerzeile**: Trennung von Header und Body
- **Body**: Daten der Anfrage

```http
### Response 1
POST https://api.openai.com/v1/chat/completions # Anfragetyp und URI
Authorization: Bearer {{$dotenv SECRET_TOKEN}}  # Header - Authentifizierung über Key
Content-Type: application/json                  # Header - Übertragender Datentyp

{
     "model": "gpt-3.5-turbo",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }
```



---

### 🤓 Weiteres

* [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) Erweiterung für VS Code
* Einfache Anfragen direkt aus dem Editor heraus

```http
GET https://api.open-meteo.com/v1/forecast?latitude=47.26&longitude=11.40&current=temperature_2m
```

* Weitere Informationen
  * [HTTP-Anfragemethoden](https://de.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP-Anfragemethoden)
  * [HTTP-Statuscode](https://de.wikipedia.org/wiki/HTTP-Statuscode)
  * OpenAPI-Spezifikationen und Swagger am Beispiel [Petstore](https://petstore3.swagger.io/)


---

### 🤓 Ausgewählte Connectoren [TF6xxx - Connectivity](https://infosys.beckhoff.com/index.php?content=../content/1031/tf6760_tc3_iot_https_rest/7611986955.html&id=)


* `TF6310 | TCP/IP`: Ermöglicht die Kommunikation über das Internetprotokoll mittels einfacher Socket-Verbindungen
* `TF6100 | OPC UA Client`: Standard für den Datenaustausch als plattformunabhängige, service-orientierte Architektur (SOA)[1]. Sie führt die Fähigkeit ein, Maschinendaten (Regelgrößen, Messwerte, Parameter usw.) nicht nur zu transportieren, sondern auch maschinenlesbar semantisch zu beschreiben.
