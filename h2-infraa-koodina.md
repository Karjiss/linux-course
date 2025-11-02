# h2 - Infraa koodina

Tämä on raportti kotitehtävään Tero Karvisen(2025) palvelin hallinta -kurssille.

-  Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: VMWare Workstation Pro

## x) Lue ja tiivistä

### **Hello Salt Infra-as-Code (Karvinen 2024)** 

- Voit kirjoittaa idempotenttiä koodia Saltin omalla kielellä ```sudoedit``` komennolla sls-tiedostoon.

### **Salt overview - Salt user guide (VMWare Inc s.a.)**

- YAML on saltin oletus merkintäkieli, jota käytetään tiedostojen käsittelyyn.
- YAML:in kolme peruselementtityyppiä ovat: skalaarit, listat ja sanakirjat.
- YAML on järjestetty lohkorakenteisiin, joissa sisennys määrittää rakenteen tason.

### **The Top File (VMWare Inc 2025)**
  
- Top on tiedosto, jonka tarkoitus on määrittää koneiden ryhmät ja niiden konfiguraatioroolit.
- Nimi "top" tulee siitä, että top-tiedostot sijaitsevat hakemistorakenteen ylimmällä tasolla.

## a) Hei infrakoodi!

Tässä tehtävässä hyödynsin Karvisen (2024) kirjoittamaa artikkelia ohjeena.

Aloitin päivittämällä Debianin komennolla: ```$ sudo apt update```.
Asensin päivityksen komennolla ```$ sudo apt upgrade```.

Kun päivitykset oli suoritettu, aloitin tehtävän teon.
Salt oli jo asennettu, joten asensin "micro" -editorin koodia varten.

Käytin komentoa: ```$ sudo apt-get -y install micro```
ja otin editorin käyttöön komennolla: ```$ export EDITOR=micro```.

Seuraavaksi loin kansion "hello" moduulille komennolla:
```$ sudo mkdir -p /srv/salt/hello/``` ja siirryin moduuliin komennolla: ```$ cd /srv/salt/hello/```.

<img width="588" height="61" alt="image" src="https://github.com/user-attachments/assets/33de110f-b05e-487a-8028-ac5b32b08991" />

- /srv/salt/ on kansio, joka jaetaan kaikille orjakoneille.

Polussa "/srv/salt/hello/" ajoin komennon: ```$ sudoedit init.sls``` kirjoittaakseni infraa koodina.

Komento avasi tekstieditorin, mihin kirjoitin kuvan mukaisen koodin.

<img width="177" height="45" alt="image" src="https://github.com/user-attachments/assets/d60fbe1e-1cd8-443f-a4a6-6f20164e4ab0" />

- Kirjoittaessa koodia tulee muistaa YAML:in säännöt.
- Tämä koodi luo kansion "hellojani" /tmp hakemistoon.
- Micro-editorissa **ctrl+q** sulkee editorin.

Sitten ajoin juuri luomani koodin komennolla: ```$ sudo salt-call --local state.apply hello```.

<img width="410" height="363" alt="image" src="https://github.com/user-attachments/assets/56c70bce-11c3-4cbd-85e5-63d7ed5c3385" />

- Tilafunktio file.managed loi kansion "hellojani" onnistuneesti.

Tarkistin, toimiko tilafunktio komennolla ```$ ls /tmp/hellojani```.

<img width="601" height="40" alt="image" src="https://github.com/user-attachments/assets/03748792-ba59-422f-9d58-82980531e75a" />



## b) Topping 

Tässä tehtävässä loin top-filen, jonka tarkoituksena oli ajaa kaikki tämänhetkiset tilat kerralla.

Aloitin tehtävän siirtymällä **/srv/salt/** hakemistoon komennolla: ```$ cd /srv/salt/```.
Loin  **top.sls**-filen ja avasin tekstieditorin komennolla: ```$ sudoedit top.sls```.

Kirjoitin tekstieditorilla koodin seuraavanlailla:
```
  base:
    '*':
      - favcli
      - hello
      - hellojani
```

Ajoin top-filen komennolla: ```$ sudo salt-call --local state.apply```.

<img width="497" height="522" alt="image" src="https://github.com/user-attachments/assets/352090ac-9153-4bc4-9ab8-a57ae3442719" />

- Top-filen ajoi kaikki määritetyt tilat onnistuneesti.
- Koodi on myös idempotentti, sillä se ei tehnyt muutoksia turhaan.

## c) Viisikko tiedostossa

Tässä tehtävässä teen viidestä tärkeimmästä Saltin tilafunktiosta erilliset esimerkit omiksi tiloikseen.

### pkg - Asentaa Apache2

Aloitin luomalla hakemiston polkuun /srv/salt/ komennolla: ```$ sudo mkdir -p /srv/salt/janipkg```.

Siirryin luomaani hakemistoon komennolla: ```$ cd /srv/salt/janipkg```, jossa loin **init.sls**-tiedoston ja avasin tekstieditorin komennolla: ```$ sudoedit init.sls```.

Kirjoitin sudoeditillä tiedostoon **init.sls**: 

```
apache2:
  pkg.installed
```

Kokeilin tilafunktion toimivuutta komennolla: ```$ sudo salt-call --local state.apply janipkg```

<img width="526" height="308" alt="image" src="https://github.com/user-attachments/assets/3f850508-8a8c-45ca-9ddc-83c77cc4c2a9" />

- Tilafunktio tarkisti, onko apache2 asennettu järjestelmälleni onnistuneesti.
- Tilafunktio on myös idempotentti, sillä se ei tee muutoksia uudelleenajaessa, ellei apache2 puutu.

### file - Luo tiedoston sisällöllä.

Seuraavaksi tein hakemiston "file" tilafunktiolle komennolla: ```$ sudo mkdir -p /srv/salt/janimorjesta```
Siirryin juuri luomaani hakemistoon, jossa loin sudoeditillä jälleen **init.sls**-tiedoston.
Kirjoitin init.sls sisälle koodin: 

```
/tmp/janimorjesta:
  file.managed:
    - contents:  'Morjesta vaan sinne'
```

Kokeilin tilaa komennolla: ```$ sudo salt-call --local state.apply janimorjesta```

<img width="399" height="363" alt="image" src="https://github.com/user-attachments/assets/ce03515e-42ef-4028-84ec-8b8333260953" />

- Tilafunktio loi tiedoston "janimorjesta" polkuun /tmp.

<img width="558" height="174" alt="image" src="https://github.com/user-attachments/assets/0b0039c5-eb83-40c3-bec5-c7dc206725e9" />

- Uudelleenajaessa muutoksia ei tule, joten tila on idempotentti.

Halusin vielä tarkistaa, syöttikö tilafunktio tiedostoon sisällön. Tarkistin sen komennolla: ```$ cat /tmp/janimorjesta```

<img width="708" height="58" alt="image" src="https://github.com/user-attachments/assets/579f75b5-a249-41f6-996e-63d84b08870b" />

- Tilafunktio toimi oikein.


### service - Käynnistää Apache2

Loin jälleen erillisen hakemiston, nyt "service" tilafunktiolle komennolla: ```$ sudo mkdir -p /srv/salt/janiservice```.

Syötin komennot kuvan mukaisesti:

<img width="653" height="78" alt="image" src="https://github.com/user-attachments/assets/a90a878e-6002-4be2-8b50-3ad9f20c7b96" />

init.sls syötettävä koodi:
```
apache2:
  service.running:
    - enable: True
```

Ajoin tilan komennolla: ```$ sudo salt-call --local state.apply janiservice```

<img width="577" height="346" alt="image" src="https://github.com/user-attachments/assets/f2c75f79-8f95-4ef5-a21b-2bbe23ecb40e" />

- Ajo suoriutui onnistuneesti
- Tilafunktio otti Apache2 käyttöön ja käynnisti sen

Uudelleenajo:

<img width="444" height="116" alt="image" src="https://github.com/user-attachments/assets/4b90566f-f5da-4215-90ea-babadbeb51b7" />

- Uudelleenajaessa muutoksia ei tullut, sillä Apache2 oli jo käynnissä.

### user - Luo uuden käyttäjän

Loin "user" tilafunktiolle hakemiston komennolla: ```$ sudo mkdir -p /srv/salt/janiuser```. 

Siirryin hakemistoon komennolla:
```$ cd /srv/salt/janiuser/```, johon loin init-tiedoston komennolla:```$ sudoedit init.sls```.

Syötin init.sls -tiedostoon koodin:
```
newuser:
  user.present:
    - home: /home/newuser
    - shell: /bin/bash
```
Kokeilin tilafunktiota komennolla: ```sudo salt-call --local state.apply janiuser```.

<img width="234" height="451" alt="image" src="https://github.com/user-attachments/assets/8ca1a91a-e512-4045-b631-bf193eb19a09" />

- Tilafunktio tarkistaa "newuser"-tunnuksen olemassa olon, sekä luo tarvittaessa uuden.
- Tilafunktio toimi onnistuneesti

<img width="329" height="113" alt="image" src="https://github.com/user-attachments/assets/404de12d-3a98-4ffd-bb86-da151e8fda66" />

- Uudelleenajaessa ei muutoksia.
- Lokissa hyvin tietoa, mitä tehtiin.

### cmd - Suorittaa komennon

Viimeiseksi tein vielä "cmd" tilafunktiolle hakemiston komennolla: ```$ sudo mkdir -p /srv/salt/janicmd```.

Siirryin hakemistoon komennolla: ```$ cd /srv/salt/janicmd/```, missä käytin komentoa:```$ sudoedit init.sls``` kirjoittaakseni koodin tilafunktiota varten.

Init.sls sisältö:
```
create-temp-file:
  cmd.run:
    - name: 'touch /tmp/ajo-filu'
    - creates: /tmp/ajo-filu
```
Ajoin tilan komennolla: ```$ sudo salt-call --local state.apply janicmd```.

<img width="313" height="365" alt="image" src="https://github.com/user-attachments/assets/1a8513c4-c813-4d23-afd5-32a5409ecae1" />

- Tilafunktio loi tiedoston "ajo-filu" /tmp hakemistoon.
- Varmistin ```$ ls /tmp/ajo-filu``` komennolla tiedoston olemassaolon.
- Uudelleenajo ei tehnyt muutoksia, sillä tiedosto oli jo olemassa.

## d) Kahden tilafunktion sls-tiedosto

Tässä tehtävässä tein sls-tiedoston, joka asentaa Apache2 ja käynnistää sen.

Muokkasin aikaisempia tehtäviä sen verran, että poistin ja sammutin apache2 tätä varten.


Ensin loin hakemiston komennolla: ```$ sudo mkdir -p /srv/salt/apacherunner```.

Siirryin apacherunner hakemistoon komennolla: ```$ cd /srv/salt/apacherunner/```.

Oikeassa hakemistossa loin sls-tiedoston tekstieditorissa komennolla: ```$ sudoedit init.sls```.

Init.sls sisältö:
```
apache2:
  pkg.installed

apache2-service:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - pkg: apache2
```

Ajoin Nämä kaksi tilafunktiota komennolla: ```$ sudo salt-call --local state.apply apacherunner```

<img width="418" height="459" alt="image" src="https://github.com/user-attachments/assets/4fc7bf9c-d698-446f-9353-d325b6087a8d" />

- Molemmat tilafunktiot suoriutuivat onnistuneesti.
- Kaksi muutosta, sekä selkeät infot lokeissa.

Uudelleenajo:

<img width="356" height="325" alt="image" src="https://github.com/user-attachments/assets/d5921a7b-9e05-49db-8eee-afc3f9ceb097" />

- Uudelleenajoissa ei ollut ollenkaan muutoksia, joten koodi on idempotenttiä.


## Lähdeluettelo

Karvinen, T. 2025. Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/

Karvinen, T. 2024. Hello Salt Infra-as-Code. Luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/

VMWare Inc. 2025. The Top File. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/top.html

VMWare Inc. s.a. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml


