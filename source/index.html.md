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
      "gebaeudeNr": "S201",
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
