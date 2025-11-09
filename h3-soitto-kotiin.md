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

## x) - Lue ja tiivistä

### **Two Machine Virtual Network With Debian 11 Bullseye and Vagrant (Karvinen 2021)**

- Vagrant automatisoi SSH-kirjautumisen.
- Voit myös luoda automaattisesti virtuaalikoneita oman tarpeen mukaan.

### **Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux (Karvinen 2018)**

- Salt masterille on tehtävä reiät portteihin: 4505/tcp sekä 4506/tcp, jos masterilla on palomuuri käytössä.
- Salt minionille tehtyjen muutosten jälkeen tulee käynnistää salt minion-palvelu uudestaan.

### **Salt Vagrant - automatically provision one master and two slaves (Karvinen 2023)**

- Top file määrittää, mille orjille ajetaan mitäkin tiloja.
- Kirjoitat infraa koodina .sls tiedostoihin käyttäen komentoja:

  ```
  $ sudo mkdir -p /srv/salt/testi
  $ sudoedit srv/salt/testi/init.sls
  ```

## a) - Hello Vagrant!

Tässä tehtävässä asensin Vagrantin, sekä VirtualBoxin. Käytin asennuksessa ohjeena HashiCorpin (s.a) nettisivuja.

### Vagrant

Suoritin Vagrantin asennuksen komennolla: 

```
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```
Varmistin vielä asennuksen komennolla: ```$ vagrant version```.

<img width="456" height="110" alt="image" src="https://github.com/user-attachments/assets/0dc7f90a-5761-411c-8a0e-ac2e759ec384" />


### VirtualBox

Virtualboxin asentamiseen Linuxille käytin VirtualBoxin lataus-sivun ohjeita (Oracle 2025).

Ensin lisäsin polun etc/apt/ tiedostoon ```sources.list``` seuraavan rivin:

``` deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian trixie contrib ```

<img width="699" height="363" alt="image" src="https://github.com/user-attachments/assets/0c3ee5c3-06e3-4bfc-8da0-514932f7d6de" />


Sitten Latasin VirtualBoxin ja lisäsin Oraclen julkisen avaimen komennolla:

```$ wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg --dearmor```

Viimeiseksi ajoin komennot: ```$ sudo apt update``` ja ```$ sudo apt-get install virtualbox-7.1``` asentaakseni VirtualBoxin.

## b) - Linux Vagrant

Tässä tehtävässä loin Vagrantilla uuden Linux-virtuaalikoneen.

Aloitin luomalla hakemiston testikoneelle komennolla: ```$ mkdir testilinux/``` ja siirryin hakemistoon komennolla: ```$ cd testilinux/```.

Sitten loin Vagrantfilen komennolla: ```$ vagrant init debian/bookworm64```.

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

Sammutin virtuaalikoneen ja etsin .vmx tiedoston tietokoneeni polusta:

```This PC/Documents/Virtual Machines/Janin Debian```

<img width="841" height="234" alt="image" src="https://github.com/user-attachments/assets/5293d0ec-c991-4178-9164-a96954f862db" />

Avasin .vmx tiedoston notepadilla ja lisäsin sinne tarvittavat rivit:

```
vhv.enable = "TRUE"
hypervisor.cpuid.v0 = "FALSE"
```

Sitten käynnistin virtuaalikoneeni taas uudelleen ja tarkistin vielä, onko KVM käynnissä komennolla: ```$ lsmod | grep kvm```.

<img width="566" height="76" alt="image" src="https://github.com/user-attachments/assets/85ae75fd-40d0-4881-9a29-edccf6720d24" />

- KVM on päällä.

Otin sen pois käytöstä komennoilla: ```$ sudo rmmod kvm_intel``` ja ```$ sudo rmmod kvm```.

Kokeilin taas ajaa komennon: ```$ vagrant up```.

<img width="877" height="443" alt="image" src="https://github.com/user-attachments/assets/d5e56e80-abbb-43ed-8cc3-9ca5bd85d3ae" />

- Ei enää erroreita!
- Ongelmat on siis ratkottu.

Poistin vielä luomani virtuaalikoneen seuraavaa tehtävää varten komennolla: ```$ vagrant destroy```.

<img width="647" height="72" alt="image" src="https://github.com/user-attachments/assets/d34f11fa-72d2-4d3d-8fd4-2851c1776a2a" />


## c) - Kaksin kaunihimpi

Tässä tehtävässä loin kahden Linux-tietokoneen verkon vagrantilla. Ohjeena käytin Karvisen (2021) artikkelia aiheesta.

Aloitin luomalla uuden hakemiston tälle operaatiolle, sekä siirryin sinne komennolla: ```$ mkdir tuplakoneet/; cd tuplakoneet/```.

Sitten lisäsin hakemistoon Vagrantfilen, jota aloin muokkaamaan komennolla: ```$ nano Vagrantfile```.

**Vagrantfile:**

```
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2019-2021 Tero Karvinen http://TeroKarvinen.com

$tscript = <<TSCRIPT
set -o verbose
apt-get update
apt-get -y install tree
echo "Done - set up test environment - https://terokarvinen.com/search/?q=vagrant"
TSCRIPT

Vagrant.configure("2") do |config|
	config.vm.synced_folder ".", "/vagrant", disabled: true
	config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
	config.vm.provision "shell", inline: $tscript
	config.vm.box = "debian/bookworm64"

	config.vm.define "t001" do |t001|
		t001.vm.hostname = "t001"
		t001.vm.network "private_network", ip: "192.168.56.3"
	end

	config.vm.define "t002", primary: true do |t002|
		t002.vm.hostname = "t002"
		t002.vm.network "private_network", ip: "192.168.56.4"
	end
	
end
```
- Vaihdoin opettajan valmiista pohjasta IP-osoitteet, sekä käyttöjärjestelmän uuteen versioon.

Vagrantfilen luonnin jälkeen kokeilin käynnistää komennolla: ```$ vagrant up```.

<img width="830" height="183" alt="image" src="https://github.com/user-attachments/assets/6f8f6fb5-53f2-476e-aea9-1f8afb1d6048" />

- Asennus onnistui.
- Aikaa meni noin 2-3 minuuttia.

Kokeilin vielä verkkoyhteyttä molemmilla tietokoneilla toisiinsa, sekä Googlen nimipalveluun:

<img width="550" height="213" alt="image" src="https://github.com/user-attachments/assets/d55ff712-000b-40b7-ad97-39848582e1b8" />

- Tietokone "t001" verkko toimii.

<img width="549" height="207" alt="image" src="https://github.com/user-attachments/assets/d7f7c10a-4f8d-417f-8feb-5efec5dbf772" />

- Tietokone "t002" verkko oli myös kunnossa.

## d) - Herra-orja verkossa.

Tässä tehtävässä asennan aikaisemmin luoduille tietokoneille Salt-minionin, sekä Salt-masterin.

Hyödynsin aikaisempaa raporttiani "h1-viisikko", jossa asensin Saltin puhtaalle Linuxille.

**Aloitin asentamalla tietokoneille Saltin seuraavalla tavalla:**

Siirryin virtuaalitietokoneille komennolla: ```$ vagrant ssh t001```ja ```$ vagrant ssh t002```.

Asensin Curlin tietokoneille komennolla: ```$ sudo apt install curl```.

Loin hakemiston julkisille avaimille komennolla: ```$ sudo mkdir -p /etc/apt/keyrings```.

Lisäsin Salt Projectin julkisen avaimen ja ohjelmalähteen määritystiedoston komennoilla:
```
# 1. Julkinen avain:
$ curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp  
# 2. Ohjelmalähteen määritystiedosto:
$ curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
```
Sitten latasin Salt-masterin "t001" tietokoneelle komennolla: ```$ sudo apt-get install salt-master```

<img width="618" height="217" alt="image" src="https://github.com/user-attachments/assets/758b5482-febf-4dc1-a795-c1a5f54b5eff" />

Tein aiemmat vaiheet myös "t002" tietokoneelle, paitsi asensin salt-minionin salt-masterin sijaan komennolla: ```$ sudo apt-get install salt-minion```

<img width="618" height="252" alt="image" src="https://github.com/user-attachments/assets/02c5ae5b-51a9-4be4-bb65-3d9a8df6cfe7" />

Seuraavaksi lisäsin "t001" IP-osoitteen uudelle riville komennolla:

```$ sudo nano /etc/salt/minion```.

Ja uusi rivi tiedostoon:

<img width="732" height="524" alt="image" src="https://github.com/user-attachments/assets/4ec9e7a8-bdc6-4408-a427-f07fea1d1046" />

- Poistin myös " #master: salt" rivin varmuuden vuoksi.

Käynnistin salt-minionin uudestaan komennolla: ```$ sudo systemctl restart salt-minion```

Salt-minionin uudelleenkäynnistyksen jälkeen siirryin master-koneelle (t001), jolla hyväksyin salt-minionin komennolla:

```$ sudo salt-key -A```

<img width="404" height="133" alt="image" src="https://github.com/user-attachments/assets/f0f4cd6e-352a-4e54-85e5-2d8d154e53b6" />

### Testi

Kokeilin herra-orja toimivuutta master-koneella komennolla:

```$ sudo salt '*' test.ping```

<img width="359" height="127" alt="image" src="https://github.com/user-attachments/assets/f9fb1d5f-715a-45de-af9d-ed9d4591ab4d" />

- Minion-tietokoneen (t002) arvo on "True", joten herra-orja arkkitehtuuri toimii oikein.

### Lopputulos

Saltin käyttöönotto onnistui, mutta olisin varmasti voinut asentaa Saltin koneille paljon helpommin käyttäen Vagrantfileä.

Halusin kuitenkin ensimmäistä kertaa Vagrantia kokeillessa asentaa kaiken manuaalisesti, mutta ensikerralla todennäköisesti automatisoin tämän vaiheen.

## e) - Kokeile vähintään kahta tilaa verkon yli

Ajoin tässä tehtävässä kaksi eri tilaa luomani verkon yli.

### pkg

Ensin otin käyttöön master-tietokoneen komennolla: ```$ vagrant ssh t001```.

Ajoin komennon: ```$ sudo salt '*' state.single pkg.installed hashcat```.

<img width="742" height="480" alt="image" src="https://github.com/user-attachments/assets/a8fb691b-04b3-4f78-afa1-b4c52012001f" />

<img width="241" height="86" alt="image" src="https://github.com/user-attachments/assets/c243ff1e-ef83-4005-9dc5-470b4ddc43b8" />


- Tila pkg.installed suoriutui onnistuneesti.
- Se latasi minionille hashcatin, sekä muut tarvittavat ohjelmat.

<img width="594" height="349" alt="image" src="https://github.com/user-attachments/assets/3da362fb-b427-4fb0-a8e5-9dddaf1e6711" />

- Uudelleenajo onnistui.
- Ei muutoksia, sillä hashcat on jo asennettu (idempotenssi).

Tarkistin vielä minion-tietokoneelta hashcatin olemassaolon erikseen.

<img width="724" height="187" alt="image" src="https://github.com/user-attachments/assets/966522e1-d6f9-434e-8636-fbd37b6e1d6a" />

### user

Ajoin master-tietokoneella user.present tilan komennolla:

```$ sudo salt '*' state.single user.present jani```

<img width="365" height="508" alt="image" src="https://github.com/user-attachments/assets/121327a9-a8d2-4de5-bd1e-dc11946262b3" />

- Tila onnistui.
- Loi uuden käyttäjän, sillä sitä ei ollut olemassa.

<img width="359" height="253" alt="image" src="https://github.com/user-attachments/assets/4173d02a-1a28-42fa-9050-f840c37fc182" />

- Uudelleenajo onnistui.
- Ei muutoksia, sillä käyttäjä on jo olemassa.
- Tila siis tarkastaa käyttäjän olemassaolon ja tarvittaessa tekee uuden.

Tarkastin vielä, onko käyttäjä luotu minion-koneelle.

Käytin minion-koneella komentoa: ```$ cat /etc/passwd```.

<img width="457" height="368" alt="image" src="https://github.com/user-attachments/assets/bd5d75ad-9deb-4de8-9175-8b362cddc503" />

- Alimpana näkyy juuri luotu käyttäjä.


# Lähdeluettelo

Hashicorp. s.a. Install Vagrant. Luettavissa: https://developer.hashicorp.com/vagrant/docs/installation

Karvinen, T. 2025. Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/

Karvinen, T. 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file

Karvinen, T. 2021. Two Machine Virtual Network with Debian 11 Bullseye and Vagrant. Luettavissa: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/

Karvinen, T. 2018. Salt Quickstart - Salt Stack Master and Slave on Ubuntu Linux. Luettavissa: https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

OpenAI. 2025. ChatGPT. Promptit näkyvät raportissa. https://chatgpt.com

Oracle. 2025. Download VirtualBox for Linux Hosts. Luettavissa: https://virtualbox.org/wiki/Linux_Downloads

