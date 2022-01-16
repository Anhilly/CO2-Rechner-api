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
* `GET */auswertung/`   

Anfragen an `*/auth`  
* `POST */auth/anmeldung`   
* `POST */auth/registrierung`   
* `POST */auth/pruefeSession`   
* `POST */auth/pruefeNutzerRolle`   
* `DELETE */auth/abmeldung`   

Anfragen an `*/db`   
* `POST */db/addFaktor`   
* `POST */db/addZaehlerdaten`   
* `POST */db/insertZaehler`   
* `POST */db/insertGebaeude`   

Anfragen an `*/mitarbeiterUmfrage`    
* `GET */mitarbeiterUmfrage/exists`    
* `GET */mitarbeiterUmfrage/mitarbeiterUmfrageForUmfrage`    
* `POST */mitarbeiterUmfrage/insertMitarbeiterUmfrage`    
* `POST */mitarbeiterUmfrage/updateMitarbeiterUmfrage`    

Anfragen an `*/umfrage`    
* `GET */umfrage/gebaeude`     
* `GET */umfrage/alleUmfragen`     
* `GET */umfrage/GetAllUmfragenForUser`     
* `GET */umfrage/GetUmfrageYear`     
* `POST */umfrage/insertUmfrage`     
* `POST */umfrage/updateUmfrage`     
* `POST */umfrage/getUmfrage`      
* `DELETE */umfrage/deleteUmfrage`     


# Auswertung

Die folgenden Endpunkte sind unter `*/auswertung`  

## GET Asuwertung
URL: `GET */auswertung?id=`

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "id": "76525192659",
    "bezeichnung": "",
    "mitarbeiteranzahl": 0,
    "jahr": 0,
    "umfragenanzahl": 0,

    "emissionenWaerme": 0.0,
    "emissionenStrom": 0.0,
    "emissionenKaelte": 0.0,
    "emissionenITGeraeteHauptverantwortlicher": 0.0,
    "emissionenITGeraeteMitarbeiter": 0.0,
    "emissionenDienstreisen": 0.0,
    "emissionenPendelwege": 0.0,
},
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 409, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```


# Authentifizierung

Die folgenden Endpunkte sind unter `*/auth`  

## Post Registrieren und Anmelden
URL zum Registrieren: `POST */auth/registrierung`

URL zum Anmelden: `POST */auth/anmeldung` 

>Request JSON

```json
{
  "username": "anton@tobi.com", //String, Email des Nutzers
  "password": "verysecurepassword" //String, Password des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "message": "Der neue Nutzeraccount wurde erstellt",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh"
},
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 500, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```

## Delete Abmelden
URL: `DELETE */auth/abmeldung`

>Request JSON

```json
{
  "username": "anton@tobi.com" //String, Email des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "message": "Der session Token wurde gelöscht"
},
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 409, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```


# Adminoberfläche: Eintragen von Daten

Die folgenden Endpunkte sind unter `*/db`  

## Post neuer CO2-Faktor für Energie
URL: `POST */db/addFaktor`

>Request JSON

```json
{
  "idEnergieversorgung": 1, //Integer
  "jahr": 2020,             //Integer
  "wert": 2000              //Integer
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": null,
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```


## Post Zählerdaten
URL: `POST */db/addZaehlerdaten`

>Request JSON

```json
{
  "pkEnergie": 1,   //Integer
  "idEnergieversorgung": 1, //Integer
  "jahr": 2020,     //Integer
  "wert": 2000.0    //float
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": null,
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```

## Post Zähler hinzufügen
URL: `POST */db/insertZaehler`

>Request JSON

```json
{
  "pkEnergie": 1,           //Integer
  "idEnergieversorgung": 1, //Integer
  "bezeichnung": "",
  "einheit": "",
  "gebaeudeRef": []         //Array an Integers
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": null,
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```

## Post Gebäude hinzufügen
URL: `POST */db/insertGebaeude`

>Request JSON

```json
{
  "nr": 1101,               //Integer
  "bezeichnung": "",
  "flaeche": {
    "hnf": 0.0,
    "nnf": 0.0,
    "ngf": 0.0,
    "ff": 0.0,
    "vf": 0.0,
    "freif": 0.0,
    "gesamtf": 0.0
  }
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": null,
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```


# Umfrage für Mitarbeiter

Die folgenden Endpunkte sind unter `*/mitarbeiterUmfrage`  

## POST Umfrage Einfügen
Hier werden die Daten der Umfrage an den Server gesendet und dort in die Datenbank eingefügt. Als Antwort wird die interne ID der neu erstellten Umfrage zurückgesendet.

Alte Version berechnet zudem die Daten für den CO2 Fußabdruck und sendet diese als antwort.

URL: `POST */mitarbeiterUmfrage/insertMitarbeiterUmfrage`

>Request JSON

```json
{
  "pendelweg": [
    {
      "strecke": 123,   //Integer, in km
      "idPendelweg": 1,  //Integer, korrespondieren mit Index in Datenbank
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

Alte Version, die auch direkt Ergebnisse zurückliefert:
```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "pendelwegeEmissionen": 12.457, //Float 
    "dienstreisenEmissionen": 42.120, //Float
    "itGeraeteEmissionen": 123456.789 //Float
  },
  "error": null
}
```

Neue Version:
```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "umfrageID": "123" // umfrage ID als string
  },
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```

## POST Umfrage Existenz Prüfen
Hier werden die Daten der Umfrage an den Server gesendet, diese berechnet die Daten für den CO2 Fußabdruck und sendet diese als antwort.

URL: `POST */mitarbeiterUmfrage/exists`

>Request JSON

```json
{
  "umfrageID": "123" // umfrage ID als string
}
```

>Response JSON im Erfolgsfall

```json
{
  "umfrageID": "123" // umfrage ID als string. Leerer String (""), wenn Umfrage nicht existiert.
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```



# Umfrage Hauptverantwortlicher

Die folgenden Endpunkte sind unter `*/umfrage`  

## POST der Umfrage

URL: `POST */umfrage/insertUmfrage`

>Request JSON

```json
{
  "bezeichnung": "name",
  "gebaeude": [
    {
      "gebaeudeNr": 1201,   //Integer
      "flaechenanteil": 35  //Integer
    }
  ],
  "anzahlMitarbeiter": 24,
  "itGeraete": [
    {
      "idITGeraete": 2, //Integer, korrespondieren mit Index in Datenbank
      "anzahl": 5
    }
  ],
  "hauptverantwortlicher": {
	"username": "anton@tobi" //String
	"sessiontoken": "545a6scasd8741dfwer" //String
  }
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "umfrageID": "123", // umfrage ID als string
    "bezeichnung": "",
},
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```

## DELETE der Umfrage

URL: `DELETE */umfrage/deleteUmfrage`

>Request JSON 

```json
{
	"umfrageID": "61cdb9e6d4ca5003d1ce75dc" //String
	"hauptverantwortlicher": {
		"username": "anton@tobi" //String
		"sessiontoken": "545a6scasd8741dfwer" //String
  }
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": null,
  "error": null
}
```

>Response JSON im Fehlerfall

```json
{
  "status": "error", //String, Request fehlgeschlagen
  "data": null,
  "error": {
    "code": 404, //Integer, Fehlercode des Response Headers
    "message": "Errormessage" //String, Errorspezifische Fehlermeldung
  }
}
```