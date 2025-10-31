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

Ajoin top-filen komennolla: ```$ sudo salt-call --local state.apply```

<img width="497" height="522" alt="image" src="https://github.com/user-attachments/assets/352090ac-9153-4bc4-9ab8-a57ae3442719" />

- Top-filen ajoi kaikki määritetyt tilat onnistuneesti.
- Koodi on myös idempotentti, sillä se ei tehnyt muutoksia turhaan.

## c) Viisikko tiedostossa


## d) Kahden tilafunktion sls-tiedosto
