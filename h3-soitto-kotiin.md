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

## a) Hello Vagrant!

Tässä tehtävässä asensin Vagrantin, sekä VirtualBoxin. Käytin asennuksessa ohjeena HashiCorpin (s.a) nettisivuja.

### Vagrant

Suoritin Vagrantin asennuksen komennolla: 

```
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```
Varmistin vielä asennuksen komennolla: ```$ vagrant version```

<img width="456" height="110" alt="image" src="https://github.com/user-attachments/assets/0dc7f90a-5761-411c-8a0e-ac2e759ec384" />


### VirtualBox

Virtualboxin asentamiseen Linuxille käytin VirtualBoxin lataus-sivun ohjeita (Oracle 2025).

Ensin lisäsin polun etc/apt/ tiedostoon ```sources.list``` seuraavan rivin:

``` deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian trixie contrib ```

<img width="699" height="363" alt="image" src="https://github.com/user-attachments/assets/0c3ee5c3-06e3-4bfc-8da0-514932f7d6de" />


Sitten Latasin VirtualBoxin ja lisäsin Oraclen julkisen avaimen komennolla:

```$ wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg --dearmor```

Viimeiseksi ajoin komennot: ```$ sudo apt update``` ja ```$ sudo apt-get install virtualbox-7.1``` asentaakseni VirtualBoxin.

## b) Linux Vagrant


