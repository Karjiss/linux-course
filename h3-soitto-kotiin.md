<img width="543" height="396" alt="image" src="https://github.com/user-attachments/assets/1cf629c5-2c97-4a7e-9fc9-78d487250a56" /># h3 - Soitto Kotiin

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

Tässä tehtävässä loin Vagrantilla uuden Linux-virtuaalikoneen.

Aloitin luomalla hakemiston testikoneelle komennolla: ```$ mkdir testilinux/``` ja siirryin hakemistoon komennolla: ```$ cd testilinux/```.

Sitten loin Vagrantfilen komennolla: ```$ vagrant init debian/bookworm64```

<img width="691" height="94" alt="image" src="https://github.com/user-attachments/assets/818918bd-a996-4809-a58c-f461f92ef8f3" />

- Vastaukseksi sain varmistuksen Vagrantfilen valmistumisesta.

Seuraavaksi kokeilin käynnistää virtuaalikonetta komennolla: ```$ vagrant up```.

<img width="726" height="172" alt="image" src="https://github.com/user-attachments/assets/efdbe784-5afa-476a-b7b7-e57650ad5c0c" />

- VBoxManage ei lähde käyntiin.
- Ongelma oli mahdollisesti virtualisointi asetuksissa.

Selvitin komennolla: ```$ egrep -c '(vmx|svm)' /proc/cpuinfo```, onko virtualisointi käytössä ja sain vastaukseksi "0".

- Virtualisointi ei siis ole käytössä

Sitten sammutin virtuaalikoneen ja avasin sen asetukset.

<img width="415" height="98" alt="image" src="https://github.com/user-attachments/assets/9799d4d6-a308-41f9-b389-3b7cd6fb7236" />

- Virtualisointi asetukset eivät olleet päällä, joten laitoin ruksin kaikkiin tyhjiin kohtiin.

Käynnistin koneen uudelleen hyväksyttyäni asetusten muutokset.

Siirryin takaisin /testilinux hakemistoon ja syötin komennon: ```$ vagrant up```.

<img width="725" height="248" alt="image" src="https://github.com/user-attachments/assets/6e7a6d62-4763-4134-90f6-2d0f7529f0d9" />

- Nyt error johtui siitä, että VT-X on jo käytössä jossain muualla.

Googletin lisää ja kyselin tekoälyltä (OpenAI) hieman vinkkejä.

ChatGPT antoi ehdotukseksi seuraavat muutokset:

Promptit:

<img width="368" height="230" alt="image" src="https://github.com/user-attachments/assets/f26b21b4-cfc8-4dc5-a396-61db20712754" />
<img width="385" height="76" alt="image" src="https://github.com/user-attachments/assets/2daa7b08-9fd6-49f1-9c3a-164c5b407037" />

Vastaus:

<img width="543" height="396" alt="image" src="https://github.com/user-attachments/assets/759a8582-d535-4d97-8950-d39a3d72f56a" />

Kokeilin tekoälyn ratkaisua.

Sammutin virtuaalikoneen ja etsin .vmx tiedoston tietokoneeni polusta: This PC/Documents/Virtual Machines/Janin Debian.

<img width="841" height="234" alt="image" src="https://github.com/user-attachments/assets/5293d0ec-c991-4178-9164-a96954f862db" />

Avasin .vmx tiedoston notepadilla ja lisäsin sinne tarvittavat rivit:

```
vhv.enable = "TRUE"
hypervisor.cpuid.v0 = "FALSE"
```

Sitten käynnistin virtuaalikoneeni taas uudelleen ja tarkistin vielä, onko KVM käynnissä komennolla: ```$ lsmod | grep kvm```.

<img width="566" height="76" alt="image" src="https://github.com/user-attachments/assets/85ae75fd-40d0-4881-9a29-edccf6720d24" />

- KVM on päällä

Otin sen pois käytöstä komennoilla: ```$ sudo rmmod kvm_intel``` ja ```$ sudo rmmod kvm```.

Kokeilin taas ajaa komennon: ```$ vagrant up```.

<img width="877" height="443" alt="image" src="https://github.com/user-attachments/assets/d5e56e80-abbb-43ed-8cc3-9ca5bd85d3ae" />

- Ei enään erroreita!
- Ongelmat siis ratkottu

