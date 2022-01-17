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
* `GET */mitarbeiterUmfrage/exists`  (kein Auth)    
* `GET */mitarbeiterUmfrage/mitarbeiterUmfrageForUmfrage`    
* `POST */mitarbeiterUmfrage/insertMitarbeiterUmfrage` (keine Auth)    
* `POST */mitarbeiterUmfrage/updateMitarbeiterUmfrage`    

Anfragen an `*/umfrage`    
* `GET */umfrage/gebaeude`     
* `GET */umfrage/alleUmfragen`     
* `GET */umfrage/GetAllUmfragenForUser`     
* `GET */umfrage/GetUmfrageYear` (keine Auth)    
* `POST */umfrage/insertUmfrage`     
* `POST */umfrage/updateUmfrage`     
* `POST */umfrage/getUmfrage`      
* `DELETE */umfrage/deleteUmfrage`     

# Format der Response

Alle gesendeten Responses folgen dem gleichen Aufbau, welche eine direkte Unterscheidung in Erfolgs- und Fehlerfall erlaubt.
Im Erfolgsfall ist das status Feld mit dem String "success" belegt und im Data Feld stehen die zurückgemeldeten Daten.
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
		"message": "Es konnte die Request verarbeitet werden"
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
{
    "id": "76525192659",
    "bezeichnung": "",
    "mitarbeiteranzahl": 20,
    "jahr": 0,
    "umfragenanzahl": 10,
	"umfrageanteil": 0.5,

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
	"vergleich4PersonenHaushalt": 0,0,
}
```


# Authentifizierung

Die folgenden Endpunkte sind unter `*/auth`  

## Anmeldung
URL: `POST */auth/anmeldung` 

>Request JSON

```json
{
  "username": "anton@tobi.com", //String, Nutzername des Nutzers
  "password": "verysecurepassword" //String, Password des Nutzers
}
```

>Response JSON im Erfolgsfall

```json
{
    "message": "Der neue Nutzeraccount wurde erstellt",
    "sessiontoken": "efjuhgsdfjh19u34z287rsdjh"
}
```

## Regestrierung
URL: `POST */auth/registrierung`

>Request JSON

```json
{
  "username": "anton@tobi.com", //String, Nutzername des Nutzers
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

## Prüfung der Nutzersession
URL: `POST */auth/pruefeSession`  

## Prüfung der Nutzerrolle
URL: `POST */auth/pruefeNutzerRolle`

## Abmeldung
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


# Eintragen von neuen Daten (Admin-Feature)

Die folgenden Endpunkte sind unter `*/db`  

## Neuer CO2-Faktor für Energieart
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


## Neuer Zählerstand für vorhanden Zähler
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


## Hinzufüngen eines Zählers
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


## Hinzufügen eines neuen Gebäudes
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


# Umfrage für Mitarbeiter

Die folgenden Endpunkte sind unter `*/mitarbeiterUmfrage`  

## Existenz einer Umfrage prüfen
URL: `GET */mitarbeiterUmfrage/exists`

JSON IST NICHT AKTUELL, DA ES EIN GET IST.
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


## Gespeicherte Mitarbeiterumfragen einer Umfrage abfragen

URL: `GET */mitarbeiterUmfrage/mitarbeiterUmfrageForUmfrage`


## Mitarbeiterumfrage einfügen
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


## Mitarbeiterumfrage ändern

URL: `POST */mitarbeiterUmfrage/updateMitarbeiterUmfrage` 


# Umfrage für Hauptverantwortlicher

Die folgenden Endpunkte sind unter `*/umfrage`  

## Alle Gebäude aus Datenbank

URL: `GET */umfrage/gebaeude`   

## Alle Umfragen aus Datenbank

URL: `GET */umfrage/alleUmfragen`     

## Alle Umfragen eines Nutzers

URL: `GET */umfrage/GetAllUmfragenForUser`     

## Bilanzierungsjahr einer Umfrage
    
Wird für Mitarbeiterumfragen verwendet, um das Bilanzierungsjahr anzuzeigen.

URL: `GET */umfrage/GetUmfrageYear`  

## Umfrage einfügen

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


## Umfrage ändern

URL: `POST */umfrage/updateUmfrage`     

## Umfrage aus Datenbank

URL: `POST */umfrage/getUmfrage`      

## Umfrage löschen

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
