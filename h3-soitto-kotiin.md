# h3 - Soitto Kotiin

Tämä on raportti kotitehtävään Tero Karvisen (2025) palvelinten hallinta kurssille.

-  Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: VMWare Workstation Pro

## x) Lue ja tiivistä

### **Two Machine Virtual Network With Debian 11 Bullseye and Vagrant (Karvinen 2021)**

- Vagrant automatisoi SSH-kirjautumisen.
- Voit myös luoda automaattisesti virtuaalikoneita oman tarpeen mukaan.

### **Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux (Karvinen 2018)**

- Salt masterille on tehtävä reiät portteihin: 4505/tcp sekä 4506/tcp, jos masterilla on palomuuri käytössä.
- Salt minionille tehtyjen muutosten jälkeen tulee käynnistää salt minion-palvelu uudestaan.

### **Salt Vagrant - automatically provision one master and two slaves (Karvinen 2023)**

- Voit tuhota virtuaalikoneen, sekä sen sisällön Vagrantilla komennolla: ```$ vagrant destroy```.
- Saltilla voit kerätä yksityiskohtaista tietoa orjakoneista.
