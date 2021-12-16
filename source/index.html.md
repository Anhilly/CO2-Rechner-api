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

# Umfrage Mitarbeiter

## POST der Umfrage
Hier werden die Daten der Umfrage an den Server gesendet, diese berechnet die Daten für den CO2 Fußabdruck und sendet diese als antwort.

URL: `POST */umfrage/mitarbeiter`

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
  ]
}
```

>Response JSON im Erfolgsfall

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

## POST der Umfrage

URL: `POST */umfrage/hauptverantwortlicher`

>Request JSON

```json
{
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
  ]
}
```

>Response JSON im Erfolgsfall

```json
{
  "status": "success", //String Request erfolgreich
  "data": {
    "kaelteEmissionen": 0.0, 
    "waermeEmissionen": 0.0, 
    "stromEmissionen": 0.0, 
    "itGeraeteEmissionen": 0.0, 
    "dienstreisenEmissionen": 0.0, 
    "pendelwegeEmissionen": 0.0 
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


# Adminoberfläche: Eintragen von Daten

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

# Authentifizierung

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