# Labo 2: Combining commands into scripts
## I/O Redirection en filters
1.
```bash
apt list --installed | less
```
2.
```bash
apt list --installed > packages.txt
```
3.
```bash
apt list --installed > packages.txt 2> dev/null
```
4.
```bash
head -11 packages.txt
```
5.
```bash
wc -l packages.txt
```
## Variabelen

Zoek de waarde op van volgende shellvariabelen. Wat betekent elke variabele?

- PATH: De lijst van directories waarin de shell zoekt naar commando's.
- HISTSIZE: Het aantal commando's dat de shell onthoudt in de history.
- UID: Het user ID van de huidige gebruiker.
- HOME: De home directory van de huidige gebruiker.
- HOSTNAME: De naam van de host.
- LANG: De taal die gebruikt wordt door de shell.
- USER: De naam van de huidige gebruiker.
- OSTYPEPWD: Het type van het besturingssysteem.

## Variabelen in scripts
1. Maak een script aan met de naam hello.sh. De eerste lijn van een script is altijd een "shebang". We gaan dat niet voor elke oefening herhalen, vanaf nu moet elk script een shebang hebben! De tweede lijn drukt de tekst "Hallo GEBRUIKER" af (met GEBRUIKER de login-naam van de gebruiker). Gebruik een geschikte variabele om de login-naam op te vragen. Maak het script uitvoerbaar en test het

De variabele met de login-naam van de gebruiker is niet gedefinieerd in het script zelf. Hoe heet dit soort variabelen?
Globale variabelen
```bash
#! /usr/bin/env bash

gebruiker=${USER}

echo "Hallo ${gebruiker}"
```


2. Maak nu een tweede script aan met de naam hey.sh. Dit script drukt "Hallo" en de waarde van de variabele ${person} af (merk op dat we deze met opzet nog niet initialiseren!). Wat zal het resultaat zijn van dit script? M.a.w. wat drukt het script af wanneer je het uitvoert? Denk eerst na voordat je het uitprobeert!

    Hallo ${person}
    Hallo gevolgd door je gebruikersnaam
    Hallo
    Het script geeft een foutboodschap omdat de variabele ${person} niet bestaat

Voeg meteen na de shebang een regel toe met het commando set -o nounset en voer het script opnieuw uit. Wat gebeurt er nu? Wat denk je dat beter is: deze regel toevoegen of niet?

Definieer de variabele ${person} op de command-line (dus NIET in het script!). Druk de waarde ervan af om te verifiëren dat deze variabele bestaat. Voer vervolgens het script uit. Werkt het nu?

Wat moet je doen om de variabele ${person} zichtbaar te maken binnen het script?

Verwijder de variabele ${person} met unset. Verifieer dat deze niet meer bestaat door de waarde op te vragen. Combineer nu eens het definiëren van deze variabele en het uitvoeren van het script in één regel, met een spatie tussen beide. De opdrachtregel heeft de vorm van: variabele=waarde ./script.sh.

Werkt het script? Kan je de variabele nog opvragen nadat het script afgelopen is?

## Filters in scripts

