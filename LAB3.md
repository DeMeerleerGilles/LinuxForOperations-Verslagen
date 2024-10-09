# Verlsag LABO 3 
Linux For Operations

## Pakketbeheer (Debian)

We verkennen de volgende taken vanuit de Linux Mint VM.

1. Installeer de volgende applicaties of "pakketten". Zorg ervoor dat je dit zowel via de grafische gebruikersinterface als vanaf de opdrachtregel kunt doen.
    - Git client
    - ShellCheck
    - VI Improved, inclusief de variant met GTK3 GUI
    - Visual Studio Code

    Installeer ook nuttige plugins zoals Markdown All In One, Markdownlint, ShellCheck, GitLens, Git Graph.

    ```bash
    sudo apt install git shellcheck vim-gtk3 code
    ```

2. Download (handmatig) het volgende pakket `sl` van het Ubuntu-systeem: [sl package](https://packages.ubuntu.com/focal/games/sl) of meer CPU-bewust van [sl package (amd64)](https://packages.ubuntu.com/focal/amd64/sl/download). Installeer dit gedownloade `.deb`-bestand vervolgens handmatig. Kun je nu het `sl`-commando in de CLI uitvoeren?
Ja

3. Kun je hetzelfde doen met het pakket `cavepacker`? [cavepacker package (amd64)](https://packages.ubuntu.com/focal/amd64/cavepacker/download), tenzij je een ander type processor hebt. Kun je `dpkg` gebruiken om informatie over dit pakket op te vragen (zie de man-pagina)? Welke problemen kom je tegen?
Nee, ik kan het niet installeren omdat het afhankelijk is van andere pakketten die niet geïnstalleerd zijn.

4. Extraheer een configuratiebestand uit een pakket.
    - Download dit pakket: [isc-dhcp-server package](http://ftp.de.debian.org/debian/pool/main/i/isc-dhcp/isc-dhcp-server_4.4.1-2.3+deb11u2_arm64.deb)
    - Waarom kan dit pakket nooit compatibel zijn met je VM?
    Het is een ARM64 pakket en mijn VM is een x86_64.
    - Kun je alle bestanden erin opsommen?
    Ja, met `dpkg -c isc-dhcp-server_4.4.1-2.3+deb11u2_arm64.deb`
    - Hoe extraheer je het `dhcpd.conf`-bestand uit dit pakket?
    Met `dpkg -x isc-dhcp-server_4.4.1-2.3+deb11u2_arm64.deb /tmp/extracted`


5. Werk de lijst met beschikbare software op je systeem bij. Toon een lijst van pakketten die moeten worden bijgewerkt. Kun je een enkel pakket bijwerken (en niet de rest van de lijst), bijvoorbeeld Firefox?
Met de volgende commando's kan je de lijst van beschikbare updates opvragen en een specifiek pakket updaten.

    ```bash
    sudo apt update
    sudo apt list --upgradable
    sudo apt upgrade firefox
    ```
