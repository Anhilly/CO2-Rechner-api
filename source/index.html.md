---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: API des CO2-Rechner der TU-Darmstadt
    content: Dokumentation für die API des CO2-Rechners der TU-Darmstadt entwickelt für das Büro für Nachhaltigkeit
---

# Introduction

Das ist die API Dokumentation für den CO2-Rechner der TU-Darmstadt.

# Überblick über API-Endpunkte

Die folgende API-Endpunkte sind aktuell im Backend definiert. Alle Endpunkte werden aus dem Frontend in mindestens einem Fetch Request aufgerufen.

Anfragen an `*/auswertung`  
* `GET */auswertung`  
* `POST */auswertung/updateSetLinkShare` 

Anfragen an `*/auth`  
* `POST */auth/anmeldung`     
* `POST */auth/registrierung`     
* `POST */auth/pruefeSession`   
* `POST */auth/passwortVergessen`
* `POST */auth/emailBestaetigung`
* `POST */auth/passwortAendern`
* `DELETE */auth/abmeldung`   

Anfragen an `*/db`   
* `POST */db/addFaktor`   
* `POST */db/addZaehlerdaten`   
* `POST */db/insertZaehler`   
* `POST */db/insertGebaeude`   

Anfragen an `*/mitarbeiterUmfrage`    
* `GET */mitarbeiterUmfrage/exists`  (kein Auth)    
* `GET */mitarbeiterUmfrage/mitarbeiterUmfrageForUmfrage`    
* `POST */mitarbeiterUmfrage/insertMitarbeiterUmfrage` (keine Auth)    
* `POST */mitarbeiterUmfrage/updateMitarbeiterUmfrage`    

Anfragen an `*/umfrage`    
* `GET */umfrage/gebaeude`     
* `GET */umfrage/alleUmfragen`     
* `GET */umfrage/GetAllUmfragenForUser`     
* `GET */umfrage/GetUmfrageYear` (keine Auth)
* `GET */umfrage/GetSharedResults` (keine Auth)    
* `POST */umfrage/insertUmfrage`     
* `POST */umfrage/updateUmfrage`     
* `POST */umfrage/getUmfrage`      
* `DELETE */umfrage/deleteUmfrage`     

# Format der Response

Alle gesendeten Responses folgen dem gleichen Aufbau, welche eine direkte Unterscheidung in Erfolgs- und Fehlerfall erlaubt.
Im Erfolgsfall ist das status Feld mit dem String "success" belegt und im data Feld stehen die zurückgemeldeten Daten.
```json
{
  "status": "success", //String Request erfolgreich
  "data": {...},
  "error": null,
}
```

Im Fehlerfall steht im status Feld der String "error" und im error Feld ist ein festes JSON Format mit Fehlermeldung und -code festgeschrieben.
```json
{
  "status": "error", //String Request fehlgeschlagen
  "data": null,
  "error": {
 	"code": 400,
	"message": "Es konnte der Request nicht verarbeitet werden",
  }
}
```

Im Nachfolgenden werden wir immer nur die data Felder auflisten.

# Auswertung

Die folgenden Endpunkte sind unter `*/auswertung`  

## Auswertung für Umfrage erstellen
URL: `GET */auswertung?id=`


>Response JSON im Erfolgsfall

```json
"data": {
    "id": "76525192659",
    "bezeichnung": "",
    "mitarbeiteranzahl": 20,
    "jahr": 0,
    "umfragenanzahl": 10,
    "umfrageanteil": 0.5,
    "auswertungFreigegeben": 0,

    "emissionenWaerme": 0.0,
    "emissionenStrom": 0.0,
    "emissionenKaelte": 0.0,
    "emissionenEnergie": 0.0,
    "emissionenITGeraeteHauptverantwortlicher": 0.0,
    "emissionenITGeraeteMitarbeiter": 0.0,
    "emissionenITGeraete": 0.0,
    "emissionenDienstreisen": 0.0,
    "emissionenPendelwege": 0.0,
    "emissionenGesamt": 0.0,
    "emissionenProMitarbeiter": 0.0,
	
    "vergleich2PersonenHaushalt": 0.0,
    "vergleich4PersonenHaushalt": 0.0,
}
```

## Link Teilen umschalten
URL: `POST */auswertung/updateSetLinkShare`

>Request JSON

```json
 {
  "umfrageID": "4a5sd48qw413",
  "freigabeWert": 0,
  "authToken": {
    "username": "anton@tobi",
    "sessiontoken": "51f86qad419d21",
  },
}
```

>Response JSON

```json
"data": null
```

# Authentifizierung

Die folgenden Endpunkte sind unter `*/auth`  

## Anmeldung
URL: `POST */auth/anmeldung` 

>Request JSON

```json
{
  "username": "testuser", //String, Nutzername des Nutzers
  "password": "verysecurepassword" //String, Password des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
"data": {
    "message": "Nutzer authentifiziert",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh" //String, Sessiontoken mit TTL
}
```

## Registrierung
URL: `POST */auth/registrierung`

>Request JSON

```json
{
  "username": "testuser", //String, Nutzername des Nutzers
  "password": "verysecurepassword" //String, Password des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
"data": {
    "message": "Der neue Nutzeraccount wurde erstellt",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh" //String, Sessiontoken mit TTL
}
```

## Prüfung der Nutzersession
URL: `POST */auth/pruefeSession`  

>Request JSON

```json
{
  "username": "testuser", //String, Nutzername des Nutzers
  "password": "verysecurepassword" //String, Password des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
"data": {
  "rolle": 0, //Int, Rolle des Nutzers, 1 fuer Admin 0 fuer User
  "emailBestaetigt": 1 // Int Ob Mail bestaetigt wurde, 1 fuer bestaetigt 0 fuer nicht bestaetigt
}
```
## Passwort Vergessen
URL: `POST */auth/passwortVergessen`  

>Request JSON

```json
{
  "username": "testuser", //String, Nutzername des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```

## E-Mail Bestaetigung versenden
URL: `POST */auth/emailBestaetigung`  

>Request JSON

```json
{
  "nutzerID": "61f47097e8b4071710362207", //String, NutzerID des Users
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```

## Passwort Aendern
URL: `POST */auth/passwortAendern`  

>Request JSON

```json
{
  "authToken": {
    "username": "anton@tobi",
    "sessiontoken": "51f86qad419d21",
  },
  "passwort": "test1234",
  "neuesPasswort": "test4321"
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```


## Abmeldung
URL: `DELETE */auth/abmeldung`

>Request JSON

```json
{
  "username": "testuser" //String, Email des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
"data": {
  "message": "Der Session Token wurde gelöscht"
}
```


# Eintragen von neuen Daten (Admin-Feature)

Die folgenden Endpunkte sind unter `*/db`  

## Neuer CO2-Faktor für Energieart
URL: `POST */db/addFaktor`

>Request JSON

```json
{
  "authToken": {
    "username": 	"testuser",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh",
  },
    "idEnergieversorgung": 1, //Integer
    "jahr": 2020,             //Integer
    "wert": 2000,             //Integer
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```


## Neuer Zählerstand für vorhanden Zähler
URL: `POST */db/addZaehlerdaten`

>Request JSON

```json
{
  "authToken": {
    "username": 	"testuser",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh",
  },
  "pkEnergie": 1,   //Integer
  "idEnergieversorgung": 1, //Integer
  "jahr": 2020,     //Integer
  "wert": 2000.0    //float
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```


## Hinzufüngen eines Zählers
URL: `POST */db/insertZaehler`

>Request JSON

```json
{
  "authToken": {
    "username": 	"testuser",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh",
  },
  "pkEnergie": 1,           //Integer
  "idEnergieversorgung": 1, //Integer
  "bezeichnung": "",
  "einheit": "",
  "gebaeudeRef": []         //Array an Integers
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```


## Hinzufügen eines neuen Gebäudes
URL: `POST */db/insertGebaeude`

>Request JSON

```json
{
  "authToken": {
    "username": 	"testuser",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh",
  },
  "nr": 1101,       //Integer
  "bezeichnung": "",
  "flaeche": {
    "hnf": 0.0,		//Float
    "nnf": 0.0, 	//Float
    "ngf": 0.0, 	//Float
    "ff": 0.0,		//Float
    "vf": 0.0,		//Float
    "freif": 0.0, 	//Float
    "gesamtf": 0.0 	//Float
  }
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```


# Umfrage für Mitarbeiter

Die folgenden Endpunkte sind unter `*/mitarbeiterUmfrage`  

## Existenz einer Umfrage prüfen
URL: `GET */mitarbeiterUmfrage/exists?id=[...]`

>Übergebene URL Parameter

* "id": "asd3as712d4as5d6d" // Umfrage ID als string


>Response JSON im Erfolgsfall

```json
"data": {
  "umfrageID": "123", // umfrage ID als string. Leerer String (""), wenn Umfrage nicht existiert.
  "bezeichnung": "Umfragename",
  "complete":  true //bool,
}
```


## Gespeicherte Mitarbeiterumfragen einer Umfrage abfragen

URL: `GET */mitarbeiterUmfrage/mitarbeiterUmfrageForUmfrage`


>Response JSON im Erfolgsfall

```json
"data": {
  "mitarbeiterUmfragen": [
    {
      "_id": "wefg3872ehadsij1asd1t",
      "pendelweg": [
        {	
          "idPendelweg": 1,  //Integer, korrespondieren mit Index in Datenbank
          "strecke": 123,   //Integer, in km
          "personenanzahl": 1   //Inetger, 1 = alleine, >1 = Fahrgemeinschaft
        }
      ],
      "tageImBuero": 7, //Integer
      "dienstreise":[
        {
          "idDienstreise": 2,  //Integer, korrespondieren mit Index in Datenbank
          "streckentyp": "Langstrecke",
          "strecke": 800, //Integer, in km
          "tankart": ""
        }
      ],
      "itGeraete": [
        {
          "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
          "anzahl": 1
        }
      ],
      "idUmfrage": "123", // umfrage ID als string
      "revision": 1 //Integer
    },
  ]
}
```

## Mitarbeiterumfrage einfügen
Hier werden die Daten der Umfrage an den Server gesendet und dort in die Datenbank eingefügt. Als Antwort wird die interne ID der neu erstellten Umfrage zurückgesendet.


URL: `POST */mitarbeiterUmfrage/insertMitarbeiterUmfrage`

>Request JSON

```json
{
  "pendelweg": [
    {
      "idPendelweg": 1,  //Integer, korrespondieren mit Index in Datenbank
      "strecke": 123,   //Integer, in km
      "personenanzahl": 1   //Inetger, 1 = alleine, >1 = Fahrgemeinschaft
    }
  ],
  "tageImBuero": 7, //Integer
  "dienstreise":[
    {
      "idDienstreise": 2,  //Integer, korrespondieren mit Index in Datenbank
      "streckentyp": "Langstrecke",
      "strecke": 800, //Integer, in km
      "tankart": ""
    }
  ],
  "itGeraete": [
    {
      "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
      "anzahl": 1
    }
  ],
  "idUmfrage": "123" // umfrage ID als string
}
```

>Response JSON im Erfolgsfall


```json
"data": null
```


## Mitarbeiterumfrage ändern

Admin only

URL: `POST */mitarbeiterUmfrage/updateMitarbeiterUmfrage` 

>Request JSON

```json
{
  "authToken": {
	"username": 	"testuser",
	"sessiontoken": "efjuhgsdfjh19u34z287rsdjh",
  },
  "pendelweg": [
    {
      "idPendelweg": 1,  //Integer, korrespondieren mit Index in Datenbank
      "strecke": 123,   //Integer, in km
      "personenanzahl": 1   //Inetger, 1 = alleine, >1 = Fahrgemeinschaft
    }
  ],
  "tageImBuero": 7, //Integer
  "dienstreise":[
    {
      "idDienstreise": 2,  //Integer, korrespondieren mit Index in Datenbank
      "streckentyp": "Langstrecke",
      "strecke": 800, //Integer, in km
      "tankart": ""
    }
  ],
  "itGeraete": [
    {
      "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
      "anzahl": 1
    }
  ],
  "idUmfrage": "123" // umfrage ID als string
}
```

>Response JSON

TODO: hier sollte null ausreichen.

```json
"data": {
  "umfrageID": "dfklj239fhab9d1320f",
  "bezeichnung": "Umfragename"
}
```


# Umfrage für Hauptverantwortlicher

Die folgenden Endpunkte sind unter `*/umfrage`  

## Alle Gebäude aus Datenbank

URL: `GET */umfrage/gebaeude`   

>Übergebene URL Parameter

Keine


>Response JSON

```json
"data": {
  "gebaeude": [ 1101, 3012 ] //Integerarray
}
```

## Alle Umfragen aus Datenbank

URL: `GET */umfrage/alleUmfragen`     

Admin only

TODO Authentication als POST

>Response JSON

```json
"data": {
  "umfragen": [ 
    {	
      "_id": "aidhb731dh9dh13jd",
      "bezeichnung": "umfragename",
      "mitarbeiteranzahl": 24, //Integer
      "jahr": 2020, //Integer
      "gebaeude": [
        {
          "gebaeudeNr": 1201,   //Integer
          "nutzflaeche": 35  //Integer
        }
      ],
      "itGeraete": [
        {
          "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
          "anzahl": 5
        }
      ],
      "auswertungFreigegeben": 0, //Integer
      "revision": 1, //Integer
      "mitarbeiterUmfrageRef": ["fh7813hd9f1j3", "fg21fg18das9d31"] //Stringarray
    }
  ]
}
```


## Alle Umfragen eines Nutzers

URL: `GET */umfrage/GetAllUmfragenForUser`     

TODO Authentication als POST

>Response JSON

```json
"data": {
  "umfragen": [ 
    {
      "_id": "aidhb731dh9dh13jd",
      "bezeichnung": "umfragename",
      "mitarbeiteranzahl": 24, //Integer
      "jahr": 2020, //Integer
      "gebaeude": [
        {
          "gebaeudeNr": 1201,   //Integer
          "nutzflaeche": 35  //Integer
		}
	  ],
	  "itGeraete": [
        {
          "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
          "anzahl": 5
        }
	  ],
    "auswertungFreigegeben": 0, //Integer
	  "revision": 1, //Integer
	  "mitarbeiterUmfrageRef": ["fh7813hd9f1j3", "fg21fg18das9d31"] //Stringarray
	}
  ]
}
```

## Bilanzierungsjahr einer Umfrage
    
Wird für Mitarbeiterumfragen verwendet, um das Bilanzierungsjahr anzuzeigen.

URL: `GET */umfrage/GetUmfrageYear?id=[...]`  

>Übergebene URL Parameter

* "id": "asd3as712d4as5d6d" // Umfrage ID als string

>Response JSON

```json
"data": {
  "jahr": 2020 // Integer
} 
```

## Auswertung zum Teilen freigegeben

Wird für das Teilen der Umfrageauswertung verwendet, um zu ermitteln ob die Auswertung durch unauthentifizierte Nutzer eingesehen werden darf.
0 bedeutet keine Freigabe, 1 Teilen aktiviert.

URL: `GET */umfrage/GetSharedResults?id=[...]`

>Übergebene URL Parameter

* "id": "asd3as712d4as5d6d" // Umfrage ID als string

>Response JSON

```json
"data": {
  "freigegeben": 0 // Integer
}
```

## Umfrage einfügen

URL: `POST */umfrage/insertUmfrage`

>Request JSON

```json
{
  "bezeichnung": "Umfragename",
  "mitarbeiteranzahl": 24, //Integer
  "jahr": 2020, //Integer
  "gebaeude": [
    {
      "gebaeudeNr": 1201,   //Integer
      "nutzflaeche": 35  //Integer
    }
  ],
  "itGeraete": [
    {
      "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
      "anzahl": 5
    }
  ],
  "authToken": {
    "username": "testuser",
    "sessiontoken": "545a6scasd8741dfwer"
  }
}
```

>Response JSON im Erfolgsfall

```json
"data": {
  "umfrageID": "asfn393f130cd1", // umfrage ID als string
  "bezeichnung": "Umfragename",
}
```


## Umfrage ändern

URL: `POST */umfrage/updateUmfrage`     

>Request JSON

```json
{
  "authToken": {
    "username": "testuser",
    "sessiontoken": "545a6scasd8741dfwer"
  },
  "umfrageID": "981jf9012fnc10ad1", // umfrage ID als String
  "bezeichnung": "Umfragename",
  "mitarbeiteranzahl": 12, //Integer
  "jahr": 2020, //Integer
  "gebaeude": [
    {
      "gebaeudeNr": 1101, //Integer korrespondiert mit ID
      "nutzflaeche": 12  //Integer
    }
  ],
  "itGeraete": [
    {
      "idITGeraete": 2, //Integer korrespondiert mit ID
      "anzahl": 12 //Integer
    }
  ],
  "revision": 1 //Integer
}
```

>Response JSON

TODO kann vermutlich auch null sein, hängt von Adminpanel ab

```json
"data": {
  "umfrageID": "981jf9012fnc10ad1",
  "bezeichnung": "Umfragename"
}
```

## Umfrage aus Datenbank

URL: `POST */umfrage/getUmfrage`      

>Request JSON

```json
{
  "umfrageID": "aidhb731dh9dh13jd",
  "authToken": {
    "username": "testuser",
    "sessiontoken": "545a6scasd8741dfwer"
  }
}
```

>Response JSON

```json
"data": {
  "_id": "aidhb731dh9dh13jd",
  "bezeichnung": "umfragename",
  "mitarbeiteranzahl": 24, //Integer
  "jahr": 2020, //Integer
  "gebaeude": [
    {
      "gebaeudeNr": 1201,   //Integer
      "nutzflaeche": 35  //Integer
    }
  ],
  "itGeraete": [
    {
      "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
      "anzahl": 5
    }
  ],
  "revision": 1, //Integer
  "mitarbeiterUmfrageRef": ["fh7813hd9f1j3", "fg21fg18das9d31"] //Stringarray
}
```

## Umfrage löschen

URL: `DELETE */umfrage/deleteUmfrage`

>Request JSON 

```json
{
  "umfrageID": "61cdb9e6d4ca5003d1ce75dc"
  "authToken": {
    "username": "testuser"
    "sessiontoken": "545a6scasd8741dfwer"
  }
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```
