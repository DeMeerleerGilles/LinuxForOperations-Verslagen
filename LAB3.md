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


## IP Configuratie

In dit deel van het labo gaan we een tweede (server-)VM aanmaken en er een LAN opzetten waar onze Linux Mint-VM en deze nieuwe deel van zullen uitmaken. Het LAN heeft als netwerk-adres 192.168.76.0/24.

### Configuratie Linux Mint VM:

1. Open in VirtualBox de VM Settings > Network > Adapter 2. Kies "Internal Network" in het dropdown-tekstvak. Klik OK om te bevestigen.
2. In de Linux Mint VM, klik rechtsonder op het icoontje voor de netwerkinstellingen (lijkt op een Ethernet-poort) en dan "Network connections". Selecteer "Wired connection 2".
3. Selecteer het tabblad "IPv4 Settings" en vul volgende informatie in:
    - Method: Manual
    - Addresses: Add, en vervolgens als Address "192.168.76.10" en Netmask "24".
    - Gateway en DNS servers vul je niet in.
4. Bevestig met "Save". Controleer dat de netwerkadapter een IP-adres heeft door in een terminal `ip a` uit te voeren.

### Configuratie van de server-VM:

Op deze VM draait een andere Linux-distributie, nl. AlmaLinux 9. Deze is afgeleid van RedHat Enterprise Linux 9 (RHEL). Om RHEL te installeren heb je in principe een (betalend) service-contract nodig bij RedHat. AlmaLinux is een gratis en volledig compatibel alternatief. RHEL (en compatibele distributies) worden beschouwd als kwalitatieve, stabiele en goed beveiligde distributies en worden daarom vaak gebruikt als server-OS.

Deze VM heeft een gebruiker "vagrant" met wachtwoord "vagrant" met sudo-privileges. Op de VM is de service `sshd` actief. Er zijn 2 netwerkkaarten. De eerste is aangesloten op een NAT-interface (en zorgt voor internettoegang), de andere is ook een "intnet" interface. De twee VMs zullen via deze laatste interface met elkaar kunnen communiceren. Je sluit deze VM best af met het commando `sudo poweroff` en niet door het VirtualBox-venster te sluiten.

1. Importeer het .ova-bestand met de server-VM in VirtualBox en start op. Je kan indien nodig de toetsenbordinstellingen aanpassen naar AZERTY met `sudo localectl set-keymap be`.
2. Pas de netwerkinstellingen aan:
    - Zorg dat de tweede netwerkinterface een vast IP-adres toegekend krijgt (nl. 192.168.76.12/24) door het gepaste configuratiebestand aan te passen.
    - Op deze netwerkinterface wordt geen default gateway of DNS-server ingesteld.
3. Pas de wijzigingen toe en controleer het effect met `ip a`.

Als beide VMs het juiste IP-adres hebben, dan zou je moeten kunnen pingen tussen de twee. Controleer dit in beide richtingen.

Reflectie: Welke voorwaarden moeten voldaan zijn zodat twee hosts op eenzelfde LAN naar elkaar kunnen pingen?

## DHCP Server

De volgende stap is nu om van de AlmaLinux-VM een DHCP-server te maken. Installeer de ISC DHCP server. Bewerk het configuratiebestand (`dhcpd.conf`) en declareer een subnet voor IP netwerk 192.168.76.0/24. Deze DHCP-server deelt dynamische IP-adressen uit vanaf 192.168.76.101 tot en met 192.168.76.253. De default lease time komt op 3u, de maximale op 7u. Eens de configuratie klaar is, start je de service op en zorg je er meteen voor dat deze ook bij booten van de VM meteen wordt opgestart.

```bash
sudo dnf install dhcp-server
sudo nano /etc/dhcp/dhcpd.conf
# Voeg de configuratie toe
sudo systemctl start dhcpd
sudo systemctl enable dhcpd
```

(Her)configureer opnieuw de tweede netwerkinterface van de Linux Mint-VM. Stel deze opnieuw in om een IP-adres via DHCP aan te vragen. Herstart de netwerkinterface en controleer of je een IP-adres krijgt en of dat overeenkomt met de DHCP-configuratie.

```bash
sudo dhclient -r
sudo dhclient
```

Je kan optioneel ook de DHCP-configuratie aanpassen om de Linux Mint-VM altijd hetzelfde gereserveerde IP-adres te geven. Dit gebeurt op basis van het MAC-adres. Stel de maximale lease time in op 24u. Na wijziging van het configuratiebestand start je de service opnieuw op en controleer je of de Linux Mint-VM het verwachte IP-adres krijgt.

```bash
sudo nano /etc/dhcp/dhcpd.conf
# Voeg de configuratie toe voor het gereserveerde IP-adres
sudo systemctl restart dhcpd
```

Wat moet je doen om ervoor te zorgen dat je Linux Mint-VM opnieuw internettoegang kan krijgen?

Een optionele uitbreiding van het labo is om van de AlmaLinux VM ook een router te maken. Deze biedt dan volwaardige internettoegang aan de Linux Mint-VM en voorziet dus alle clients niet alleen van een IP-adres, maar ook van een default gateway en DNS-server. Om (NAT) routering aan te zetten op de AlmaLinux-VM, voer je volgende stappen uit:

1. Bewerk `/etc/sysctl.conf` en voeg een lijn toe met: `net.ipv4.ip_forward=1`
2. Voer (als root) `sysctl -p` uit

```bash
sudo nano /etc/sysctl.conf
# Voeg de lijn toe
sudo sysctl -p
```

De VM is nu een router, maar je moet ook nog NAT aanzetten. Dat kan met volgende commando's (als root):

```bash
sudo nft add table nat
sudo nft 'add chain nat postrouting { type nat hook postrouting priority 100 ; }'
sudo nft add rule nat postrouting masquerade
```
