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
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Das ist die API Dokumentation für den CO2-Rechner der TU-Darmstadt.

# Umfrage Mitarbeiter

## POST der Umfrage
Hier werden die Daten der Umfrage an den Server gesendet, diese berechnet die Daten für den CO2 Fußabdruck und sendet diese als antwort.
>Request JSON

```json
{
  "pendelweg": [
    {
      "strecke": 123,   //Integer, in km
      "idPendelweg": 1  //Integer, korrespondieren mit Index in Datenbank
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

>Response JSON

```json
()
```

# Umfrage Hauptverantwortlicher

## POST der Umfrage
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

>Response JSON

```json
{
  "kaelteEmissionen": 0.0,
  "waermeEmissionen": 0.0,
  "stromEmissionen": 0.0,
  "itGeraeteEmissionen": 0.0,
  "dienstreisenEmissionen": 0.0,
  "pendelwegeEmissionen": 0.0
}
```


# Adminoberfläche: Eintragen von Daten

URL: example.de/db/*

## Post neuer CO2-Faktor für Energie
URL: example.de/db/addFaktor

>Request JSON

```json
{
  "idEnergieversorgung": 1, //Integer
  "jahr": 2020,             //Integer
  "wert": 2000              //Integer
}
```

## Post Zählerdaten
URL: example.de/db/addZaehlerdaten

>Request JSON

```json
{
  "pkEnergie": 1,   //Integer
  "jahr": 2020,     //Integer
  "wert": 2000.0    //float
}
```

## Post Zähler hinzufügen
URL: example.de/db/insertZaehler

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

## Post Gebäude hinzufügen
URL: example.de/db/insertGebaeude

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
