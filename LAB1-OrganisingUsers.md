# Verslag Labo 1: organising users

Linux For Operations

## Gebruikers aanmaken

1. Wat is het commando om de huidige directory op te vragen? In welke map bevind je jou nu?
   (print workung directory)

```bash
pwd
```

1. Wat is het UID van deze gebruiker, wat is de GID?

```bash
id
```

Het UID van de gebruiker hogent is 1001 en de GID is 1001

2. Maak een nieuwe gebruiker aan met de naam alice, zonder specifieke opties. Werk hiervoor met adduser.

```bash
adduser alice
```

3. In welk bestand kan je de UID, gebruikersnaam, homedirectory, enz. van alle gebruikers terugvinden?

```bash
less /etc/passwd
```

4. In welk configuratiebestand kan je al de bestaande gebruikersgroepen nakijken, en ook de gebruikers die lid zijn van elke groep?

```bash
less /etc/group
```

5. In welk configuratiebestand vind je de wachtwoorden van alle gebruikers?

```bash
less /etc/shadow
```

## Gebruikersgroepen aanmaken

1. Maak een groep aan met de naam sporten

```bash
addgroup sporten
```

2. In welk configuratiebestand vind je het GID van deze groep terug?

```bash
less /etc/group
```

3. Wat zal het GID zijn van de groepen zwemmen en judo als je deze nu onmiddellijk zou aanmaken? Maak ze aan en controleer!

```bash
addgroup zwemmen
addgroup judo
```

4. Voeg de gebruiker alice toe aan de groepen sporten en zwemmen

```bash
usermod -aG sporten alice
usermod -aG zwemmen alice
```

5. Log in als alice door in een terminal het commando su - alice (let op de spaties!) uit te voeren

```bash
su - alice
```

6. Wat is de home-directory van alice?

```bash
pwd
```

7. Zorg er nu voor dat de groep sporten de primaire groep wordt van alice.

```bash
usermod -g sporten alice
```

8. Zorg er voor dat alice uitgelogd is, ga terug naar root

```bash
exit
```

## Extra gebruikers aanmaken

Maak nu de gebruikers in onderstaande tabel aan. Zorg er voor dat ze al meteen bij aanmaken tot de aangegeven groepen behoren. Kies zelf geschikte wachtwoorden voor deze gebruikers en vergeet ze niet (vul eventueel een kolom toe aan de tabel).

| Gebruikersnaam | Primaire groep | Secundaire groep |
| -------------- | -------------- | ---------------- |
| bob            | sporten        | judo             |
| carol          | sporten        | zwemmen          |
| daniel         | sporten        | judo             |
| eva            | sporten        | zwemmen          |

1. Geef de gebruikte commando's om de gebruikers aan te maken en ook om te verifiëren of dit correct gebeurd is:

```bash
adduser bob
usermod -aG sporten bob
usermod -aG judo bob

adduser carol
usermod -aG sporten carol
usermod -aG zwemmen carol

adduser daniel
usermod -aG sporten daniel
usermod -aG judo daniel

adduser eva
usermod -aG sporten eva
usermod -aG zwemmen eva
```

2. Verwijder nu de groep alice en controleer.

```bash
deluser alice
```

3. Verwijder alice uit de groep zwemmen - of liever: zorg dat ze enkel nog in de groep sporten zit.

```bash
gpasswd -d alice zwemmen
```

4. Gebruiker daniel gaat een tijdje niet meer sporten. Zorg er voor dat deze gebruiker tot nader order geen toegang meer kan hebben tot het systeem (zonder het wachtwoord of de gebruiker te verwijderen!).

```bash
passwd -l daniel
```

5. Hoe kan je controleren dat daniel inderdaad geen toegang meer heeft tot het systeem? In welk bestand kan dat en hoe zie je daar dan dat het account afgesloten is?

```bash
less /etc/shadow
```

6. Gebruiker daniel komt terug naar de sportclub. Geef hem opnieuw toegang tot het systeem.

```bash
passwd -u daniel
```

7. Gebruiker eva stopt helemaal met sporten. Verwijder deze gebruiker, maar doe dit zorgvuldig: zorg er in het bijzonder voor dat ook haar homedirectory verwijderd wordt.

```bash
deluser --remove-home eva
```

8. Log aan als de gebruiker carol

```bash
su - carol
```

- Controleer of je in de “thuismap” bent van deze gebruiker.


    ```bash
    pwd
    ```

- Maak onder deze map een bestand test aan door middel van het commando touch.
  ```bash
  touch test
  ```
- Probeer nu als gebruiker carol je te verplaatsen naar de “thuismap” van alice.

```bash
cd /home/alice
```

- Kan je de inhoud van de mappen binnen de thuismap van alice bekijken?

```bash
 ls
```

- Probeer nu als carol onder de “thuismap” van alice ook een bestand test te maken. Lukt dit? Kan je dit verklaren?

```bash
touch test
```

## Werken als root

1. Bekijk de eerste regels van het bestand /etc/shadow. Wat bemerk je bij de gebruiker root?

```bash
less /etc/shadow
```

2. Log in als de root-gebruiker met het commando sudo -i (let op de spatie!)

```bash
sudo -i
```

- Wat is de home-directory van root?

- Wat is het UID van deze gebruiker, wat is de GID?

1. Stel, nog steeds ingelogd als root, een wachtwoord in voor root. Kies een uniek, nieuw wachtwoord! Merk je de verandering in /etc/shadow?
2. Log in als de root-gebruiker met het commando su - (let op de spatie!)
3. Log uit, en log opnieuw in met sudo su -. Wat wordt er anders?

## Jezelf toevoegen en admin maken

1. Voeg jezelf (met je eigen gekozen loginnaam) toe aan de VM waarin we werken. Gebruik hiervoor useradd -m
2. Bewerk /etc/passwd zodat je ook bash gebruikt als default shell.
3. Bekijk /etc/shadow. Bemerk dat jijzelf als gebruiker nog geen wachtwoord hebt! Stel het in met passwd
4. Nu wil je jezelf eveneens adminstrator van het systeem maken, zodat je met sudo beheerstaken kan uitvoeren. Aan welke groep voeg je jezelf hiervoor toe (gezien je niet op RHEL aan het werken bent)?
   Hint: https://www.ibm.com/docs/en/cabi/1.1.4?topic=premises-configuring-sudo-access-users

## Eigenaars en groepseigenaars veranderen

1. Je maakte hierboven reeds 2 groepen aan met de namen zwemmen en judo. Maak als root onder /srv/ twee directories aan met de naam groep/zwemmen/ en groep/judo/. Zorg dat de groepen eigenaar zijn van de overeenkomstige directories en dat carol eigenaar is van directory zwemmen en bob van het directory judo. Geef de gebruikte commando’s en controleer:

```bash
# ls -l groep/
total 8
drwxr-xr-x 2 bob   judo    4096 Sep 24 21:32 judo
drwxr-xr-x 2 carol zwemmen 4096 Sep 24 21:32 zwemmen
```

2. Zorg ervoor dat gebruikers en groepen uit de vorige stap alle permissies hebben. Geef het geschikte commando en controleer.

```bash
chmod 777 groep/
```

3. Voeg een andere gebruiker, vb daniel, toe aan zowel de groep zwemmen en judo en controleer. Geen van beide groepen zijn primair.

```bash
usermod -aG zwemmen daniel
usermod -aG judo daniel
```

4. Log in als daniel en ga naar de directory zwemmen. Laat de gebruiker hier een leeg bestand, bestand1, aanmaken in de directory zwemmen. (Indien je hier problemen ondervindt, log dan in via een andere terminalvenster).

```bash
su - daniel
cd /srv/groep/zwemmen
touch bestand1
```

5. Wie is nu eigenaar van bestand1 en wie de groepseigenaar?

```bash
ls -l
```

6. Zorg er nu voor dat de groepseigenaar van de directory zwemmen automatisch de groepseigenaar wordt van alle bestanden en directories die onder zwemmengemaakt worden. Doe hetzelfde voor de directory judo.
   Geef de gebruikte commando’s.

```bash
chmod g+s zwemmen
chmod g+s judo
```

1. Verander opnieuw naar gebruiker daniel en laat deze gebruiker een leeg bestand2 aanmaken in de directory zwemmen. Geef de gebruikte commando’s.

```bash
su - daniel
cd /srv/groep/zwemmen
touch bestand2
```

2. Wie is nu eigenaar van bestand2 en wie groepseigenaar?

```bash
ls -l
```

3. Laat nu gebruiker carol een leeg bestand bestand3 aanmaken. Controleer de eigenaar van bestand3 en de groepseigenaar.

```bash
su - carol
cd /srv/groep/judo
touch bestand3
ls -l
```

4.  Laat nu gebruiker daniel bestand3 verwijderen. Lukt dit?

```bash
rm /srv/groep/judo/bestand3
```

5.  Zorg er nu voor dat de gebruikers elkaars bestanden niet kunnen verwijderen. Als de gebruiker echter eigenaar is van het betreffende bestand mag dit wel. Leg uit hoe je dit doet en controleer. Schrijf je gevolgde procedure op.

```bash
chmod o-t /srv/groep/judo
chmod o-t /srv/groep/zwemmen
```

## Reflectievragen

1. Je kan inloggen op het systeem als root met de volgende twee commando's:

```bash
su -

sudo su -
```

Beide manieren leveren je root access op, maar de wachtwoorden kunnen verschillend zijn. Wiens wachtwoord gebruik je bij het eerste commando, wiens bij het tweede?

1. Geef drie manieren om een gebruiker (tijdelijk) de toegang tot het systeem te ontzeggen.

```bash
passwd -l daniel
usermod -L daniel
usermod -s /sbin/nologin daniel
```
