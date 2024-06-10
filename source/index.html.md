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

Die folgende API-Endpunkte sind aktuell im Backend definiert. 

Anfragen an `*/`
* `GET */`            (keine Auth)
* `Get */authRoute`

Anfragen an `*/auswertung`  
* `GET */auswertung`  (special auth)
* `POST */auswertung/updateLinkShare` 

Anfragen an `*/db`   
* `POST */db/addFaktor`                 (admin auth)
* `POST */db/addZaehlerdaten`           (admin auth)
* `POST */db/addZaehlerdatenCSV`        (admin auth)
* `POST */db/addStandardZaehlerdaten`   (admin auth)
* `POST */db/addVersorger`              (admin auth)
* `POST */db/addStandardVersorger`      (admin auth)
* `POST */db/insertZaehler`             (admin auth)
* `POST */db/insertGebaeude`            (admin auth)

Anfragen an `*/nutzer`
* `GET */nutzer/pruefeNutzer`
* `GET */nutzer/rolle`
* `DELETE */nutzer`

Anfragen an `*/mitarbeiterumfrage`    
* `GET */mitarbeiterumfrage/exists`  (kein Auth)    
* `GET */mitarbeiterumfrage/mitarbeiterumfrageFuerUmfrage`  (nicht genutzt, admin auth)
* `POST */mitarbeiterumfrage/insert` (keine Auth)    

Anfragen an `*/umfrage`    
* `GET */umfrage`
* `GET */umfrage/duplicate` 
* `GET */umfrage/jahr`              (keine Auth)
* `GET */umfrage/sharedResults`     (keine Auth)    
* `GET */umfrage/gebaeude`
* `GET */umfrage/gebaeudeUndZaehler` 
* `GET */umfrage/alleUmfragen`      (admin auth)
* `GET */umfrage/alleUmfragenVonUser`
* `POST */umfrage/insert`     
* `POST */umfrage/update`      
* `DELETE */umfrage`     

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

# Allgemeine Testendpunkte 

Die folgenden Endpunkte sind unter `*/` und dienen dazu die Erreichbarkeit des Backends zu testen.

## Testendpunkt ohne Authentifikation
URL: `GET */`

>Response im Erfolgsfall nur HTTP Status Code 200.


## Testendpunkt mit Authentifikation
URL: `Get */authRoute`

>Response im Erfolgsfall nur HTTP Status Code 200.


# Auswertung

Die folgenden Endpunkte sind unter `*/auswertung`  

## Auswertung für Umfrage erstellen
URL: `GET */auswertung?id=[umfrageID]`

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
    "emissionenDienstreisenAufgeteilt": {   // Map String to Double
      "name": 0.0
    },
    "emissionenPendelwegeAufgeteilt": {     // Map Int to Double
      1: 0.0
    },
    "emissionenITGeraeteAufgeteilt": {      // Map Int to Double
      1: 0.0
    },
    
    "verbrauchStrom": 0.0,
    "verbrauchWaerme": 0.0,
    "verbrauchKaelte": 0.0,
    "verbrauchEnergie": 0.0,
	
    "vergleich2PersonenHaushalt": 0.0,
    "vergleich4PersonenHaushalt": 0.0,

    "gebauedeIDsUndZaehler": [ 
      {	
        "nr": 0,              // Integer
        "kaelteRef": [0, 0],  // Integer-Array
        "waermeRef": [0, 0],  // Integer-Array
        "stromRef": [0, 0],   // Integer-Array
      }
    ],
    "zaehler": [
      {
        "pkEnergie": 0,         // Integer
        "zaehlerdatenVorhanden": [
          {
            "jahr": 2000,       // Integer
            "vorhanden": true,  // Boolean
          }
        ],
      }
    ],
    "umfrageGebaeude": [
      {
        "gebaeudeNr": 0,
        "nutzflaeche": 0,
      }
    ]
}
```

## Link Teilen umschalten
URL: `POST */auswertung/updateLinkShare`

>Request JSON

```json
 {
  "umfrageID": "4a5sd48qw413",
  "freigabeWert": 0,
}
```

>Response JSON

```json
"data": null
```

# Eintragen von neuen Daten (Admin-Feature)

Die folgenden Endpunkte sind unter `*/db`  

## Neuer CO2-Faktor für Energieart
URL: `POST */db/addFaktor`

>Request JSON

```json
{
    "idEnergieversorgung": 1, //Integer
    "idVertrag": 1,           //Integer
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

## Standard Zählerstand für alle vorhanden Zähler
URL: `POST */db/addStandardZaehlerdaten`

>Request JSON

```json
{
  "jahr": 2020,     //Integer
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```

## Mehrere Zählerstand über CSV-Datei
URL: `POST */db/addZaehlerdatenCSV`

>Request JSON

```json
{
  "pkEnergie": [1],           //Array of Integer
  "idEnergieversorgung": [1], //Array of Integer
  "jahr": 2020,               //Integer
  "wert": [2000.0]            //Array of float
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```

## Neuen Versorger für vorhandenes Gebäude
URL: `POST */db/addVersorger`

>Request JSON

```json
{
  "nr": 1,                  //Integer
  "idEnergieversorgung": 1, //Integer
  "jahr": 2020,             //Integer
  "idVertrag": 1            //Integer
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```

## Standard Versorger für alle vorhanden Zähler
URL: `POST */db/addStandardVersorger`

>Request JSON

```json
{
  "jahr": 2020,     //Integer
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
  },
  "waerme_versorger_jahre": [1],  //Array of Integer
  "kaelte_versorger_jahre": [1],  //Array of Integer
  "strom_versorger_jahre": [1],   //Array of Integer
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```


# Nutzerdaten

Die folgenden Endpunkte sind unter `*/nutzer`

## Existenz eines Nutzers prüfen
Prüft, ob ein Nutzer in der Datenbank schon existiert, oder ob ein Nutzer migriert werden kann. Andernfalls wird ein neuer Nutzer erstellt. Server verwendet Daten aus den Auth Token.

URL: `GET */nutzer/pruefeNutzer`

>Response JSON im Erfolgsfall

```json
"data": null
```

## Liefert Rolle eines Nutzers
URL: `GET */nutzer/rolle`

>Response JSON im Erfolgsfall

```json
"data": {
  "rolle": 0, //Integer
}
```

## Löscht einen Nutzer
URL: `DELETE */nutzer`

>Request JSON

```json
{
  "username": "xyz",       //String
}
```

>Response JSON im Erfolgsfall

```json
"data": "xyz"
```


# Umfrage für Mitarbeiter

Die folgenden Endpunkte sind unter `*/mitarbeiterumfrage`  

## Existenz einer Umfrage prüfen
URL: `GET */mitarbeiterumfrage/exists?id=[umfrageID]`

>Übergebene URL Parameter

* "id": "asd3as712d4as5d6d" // Umfrage ID als string


>Response JSON im Erfolgsfall

```json
"data": {
  "umfrageID": "123", // umfrage ID als string. Leerer String (""), wenn Umfrage nicht existiert.
  "bezeichnung": "Umfragename",
  "complete":  true, //bool
}
```


## Gespeicherte Mitarbeiterumfragen einer Umfrage abfragen

URL: `GET */mitarbeiterumfrage/mitarbeiterumfrageFuerUmfrage?id=[umfrageID]`

>Übergebene URL Parameter

* "id": "asd3as712d4as5d6d" // Umfrage ID als string

>Response JSON im Erfolgsfall

```json
"data": {
  "mitarbeiterumfragen": [
    {
      "_id": "wefg3872ehadsij1asd1t",
      "pendelweg": [
        {	
          "idPendelweg": 1,  //Integer, korrespondieren mit Index in Datenbank
          "strecke": 123,   //Integer, in km
          "personenanzahl": 1   //Integer, 1 = alleine, >1 = Fahrgemeinschaft
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


URL: `POST */mitarbeiterumfrage/insert`

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

# Umfrage für Hauptverantwortlicher

Die folgenden Endpunkte sind unter `*/umfrage`  


## Bilanzierungsjahr einer Umfrage
    
Wird für Mitarbeiterumfragen verwendet, um das Bilanzierungsjahr anzuzeigen.

URL: `GET */umfrage/jahr?id=[umfrageID]`  

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

URL: `GET */umfrage/sharedResults?id=[...]`

>Übergebene URL Parameter

* "id": "asd3as712d4as5d6d" // Umfrage ID als string

>Response JSON

```json
"data": {
  "freigegeben": 0 // Integer
}
```

## Alle Gebäude aus Datenbank

URL: `GET */umfrage/gebaeude`   

>Response JSON

```json
"data": {
  "gebaeude": [ 1101, 3012 ] //Integerarray
}
```

## Alle Gebäude und Zähler aus Datenbank

URL: `GET */umfrage/gebaeudeUndZaehler`   

>Response JSON

```json
"data": {
  "gebaeude": [ 
    {	
      "nr": 0,              // Integer
      "kaelteRef": [0, 0],  // Integer-Array
      "waermeRef": [0, 0],  // Integer-Array
      "stromRef": [0, 0],   // Integer-Array
    }
  ],
  "zaehler": [
    {
      "pkEnergie": 0,         // Integer
      "zaehlerdatenVorhanden": [
        {
          "jahr": 2000,       // Integer
          "vorhanden": true,  // Boolean
        }
      ],
    }
  ]
}
```


## Alle Umfragen aus Datenbank

URL: `GET */umfrage/alleUmfragen`     

Admin only

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

URL: `GET */umfrage/allUmfragenVonUser`     

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

## Umfrage einfügen

URL: `POST */umfrage/insert`

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

URL: `POST */umfrage/update`     

>Request JSON

```json
{
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

URL: `GET */umfrage?id=[umfrageID]`      

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

URL: `DELETE */umfrage`

>Request JSON 

```json
{
  "umfrageID": "61cdb9e6d4ca5003d1ce75dc"
}
```

>Response JSON im Erfolgsfall

```json
"data": null
```

## Umfrage ID einen Nutzer hinzufügen

URL: `POST */umfrage/share`

>Request JSON 

```json
{
  "umfrageID": "61cdb9e6d4ca5003d1ce75dc"
}
```

>Response JSON im Erfolgsfall

```json
"data": {
  "bezeichnung": "umfrage",   // String
  "hinzugefuegt": true        // Bool
}
```

## Umfrage duplizieren

URL: `POST */umfrage/duplicate`

>Request JSON 

```json
{
  "umfrageID": "61cdb9e6d4ca5003d1ce75dc",
  "suffix": "(Kopie)"
}
```

>Response JSON im Erfolgsfall

```json
"data": {
  "umfrageID": "61cdb9e6d4ca5003d1ce75dc"
}
```
