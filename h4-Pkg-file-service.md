# h4 - Pkg-file-service

Tämä on raportti kotitehtävään Tero Karvisen (2025) palvelinten hallinta kurssille.

-  Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: **VMWare Workstation Pro** ja **Vagrant**

## x) - Lue ja tiivistä

### Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port (Karvinen 2018)

- "Package-file-service" tilalla voidaan automatisoida SSH-konfigurointi ja ylläpito.
- Voit hallita tuhansia ohjelmia tällä tilalla.
- Jos SSH vastaa porttiin "**8888**", tila toimii.

## a) SSHouto

Tässä tehtävässä käytin pkg-file-serviceä SSH asennukseen, sekä käyttöönottoon herra-orja arkkitehtuurissa.

Aloitin tehtävän kokeilemalla manuaalisesti SSH:n asennusta.

Syötin master-koneella komennon: ```$ sudo apt-get install ssh```.

<img width="708" height="281" alt="image" src="https://github.com/user-attachments/assets/512c53e7-1470-43f7-9778-0eb584249c3d" />

Avasin SSH:n konfigurointitiedoston komennolla: ```$ sudoedit /etc/ssh/sshd_config```, jonne lisäsin rivin "Port 8888".

<img width="726" height="436" alt="image" src="https://github.com/user-attachments/assets/9820757c-f8c8-4a8f-a0e8-c8bf8a6b7def" />


