# REST – Representational State Transfer

## Client-Server-Architektur

- **Client** sendet **Anfragen**
- **Server**: Bearbeitet Anfragen und sendet **Antworten**

Im Gegensatz zu MQTT kommunizieren Client und Server direkt miteinander (**Peer-to-Peer**) — kein Broker dazwischen.

---

## Hypertext Transfer Protocol (HTTP)

- **HTTP** ist ein Protokoll auf Anwendungsschicht
- Bekannt für die Übertragung von Webseiten, aber auch für alle anderen Daten geeignet (sofern Stateless)
- Unter den meisten Betriebssystemen verfügbar (`cURL`)

*Website über PowerShell:*
```ps
curl "www.google.com"
```

*[Wetter-API](https://open-meteo.com/en/docs) über PowerShell:*
```ps
curl "https://api.open-meteo.com/v1/forecast?latitude=47.26&longitude=11.40&current=temperature_2m"
```

*Allgemeine Syntax:*
```ps
curl "<URL>/?<query_parameter>"
```

---

## Adressierung über Uniform Resource Locator (URL)

- Identifiziert und lokalisiert eine Ressource über das verwendete Netzwerkprotokoll (z.B. HTTP oder FTP)
- `host` kann auch eine IP-Adresse sein

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

## Antwort auf HTTP-Request

*[Wetter-API](https://open-meteo.com/en/docs) über PowerShell:*
```ps
curl "https://api.open-meteo.com/v1/forecast?latitude=47.26&longitude=11.40&current=temperature_2m"
```

```
StatusCode        : 200
StatusDescription : OK
Content           : {"latitude":47.260002,"longitude":11.4,"generationtime_ms...
Forms             : {}
Headers           : {[Transfer-Encoding, chunked], [Connection, keep-alive], [Content-Type, application/json; charset=utf-8], [Date...
```

Neben den Daten (`Content`) enthält die Antwort auch Metadaten (`Header`, `StatusCode`, ...).

---

## HTTP mit Python – Client (`requests`)

Zur Kommunikation mit REST-Services wird das `requests`-Modul verwendet:

```python
import requests

response = requests.get('https://www.mci.edu/de/')

print(response.status_code)              # 200 → alles OK
print(response.headers['content-type'])  # text/html; charset=utf-8
print(response.text)                     # <!doctype html> ...
```

---

## HTTP mit Python – Server (`flask`)

Zum Anbieten eines REST-Services wird das `flask`-Modul verwendet:

```python
from flask import Flask, request
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

@app.route('/number_of_bottles', methods=['GET', 'POST'])
def number_of_bottles():
    if request.method == 'POST':
        return f"Number of bottles changed to {request.data.decode('utf-8')}", 200
    elif request.method == 'GET':
        return "42", 200
    else:
        return "Method not allowed", 405
```

Starten:
```ps
$env:FLASK_APP = "hello.py"
python -m flask run
# * Running on http://127.0.0.1:5000/
```

---

## HTTP-Methoden

### GET

Fordert eine Ressource vom Server an — **keine Nebeneffekte**, der Serverzustand wird nicht verändert.

```python
import requests

response = requests.get(url='http://api.citybik.es/v2/networks/stadtrad-innsbruck')
# > {"network":{"company":["Nextbike GmbH"],"href":"/v2/networks/stadtrad-innsbruck",...
```

### POST

Fügt eine neue Ressource ein. Als Ergebnis wird der neue Ressourcenlink zurückgegeben.

```python
my_data = """{
  "contact": {
      "id": "1",
      "firstName": "Julian",
      "lastName": "Huber"
  }
}"""

response = requests.post(
    url='https://3d3m9.mocklab.io/v1/contacts',
    data=my_data,
    headers={'Content-Type': 'application/json'}
)
print(response.text)  # > Created a new contact!
```

---

## Struktur einer HTTP-Anfrage

- **Request-Line**: Methode, URI und Protokollversion
- **Header**: Metadaten zur Anfrage, z.B. Authentifizierung
- **Leerzeile**: Trennung von Header und Body
- **Body**: Daten der Anfrage

```http
POST https://api.openai.com/v1/chat/completions
Authorization: Bearer {{$dotenv SECRET_TOKEN}}
Content-Type: application/json

{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Say this is a test!"}],
    "temperature": 0.7
}
```

---

## Weiterführendes

- [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) Erweiterung für VS Code — einfache Anfragen direkt aus dem Editor:

```http
GET https://api.open-meteo.com/v1/forecast?latitude=47.26&longitude=11.40&current=temperature_2m
```

- [HTTP-Anfragemethoden (Wikipedia)](https://de.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP-Anfragemethoden)
- [HTTP-Statuscodes (Wikipedia)](https://de.wikipedia.org/wiki/HTTP-Statuscode)
- OpenAPI-Spezifikationen und Swagger am Beispiel [Petstore](https://petstore3.swagger.io/)

---

## TwinCAT Connectivity (🤓)

Beckhoff bietet verschiedene [TF6xxx-Konnektoren](https://infosys.beckhoff.com/index.php?content=../content/1031/tf6760_tc3_iot_https_rest/7611986955.html&id=) für die Kommunikation aus der SPS:

- `TF6310 | TCP/IP`: Kommunikation über das Internetprotokoll mittels einfacher Socket-Verbindungen
- `TF6100 | OPC UA Client`: Plattformunabhängiger Datenaustausch mit maschinenlesbarer semantischer Beschreibung
