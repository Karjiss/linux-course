# h1 - Viisikko

- Kurssi: Palvelinten Hallinta (Karvinen 2025)
- Opettaja: Tero Karvinen
- Raportin kirjoittaja: Jani Karjalainen

Tässä kotitehtävässä kerron tiivistetysti muutamasta artikkelista ranskalaisin viivoin. Seuraavassa tehtävässä asennan Linuxin ja sen sisälle Saltin (salt-minion).
Käyn myös läpi esimerkeillä Saltin tärkeimpiä tilafunktioita, sekä kerron hieman termistä "idempotentti".

  ## x) Lue ja tiivistä

  - **Saltin tärkeimmät tilafunktiot ovat: pkg, file, service, user ja cmd (Karvinen 2023).**
  
    Nämä olivat uusia asioita minulle, mutta todella hyödyllisiä hallintatyökaluja.
    
  - **Voit hallinnoida tuhansia tietokoneita käyttäen Salttia (Karvinen 2018).**

    Mietinkin tietoturvasta kiinnostuneena, että käytetäänkö tätä paljon kyberhyökkäyksissä? Todennäköisesti.
    
  - **Asentaessa onedir version Saltista, Salt asentaa myös oman paikallisen version Pythonista, sekä muut toimivuuden kannalta tarvittavat riippuvuudet (VMWare Inc. s.a.).**
    
  - **Orjatietokoneen täytyy tietää, missä isäntä on. Orjatietokoneilla tulee myös olla uniikit tunnukset (Karvinen 2018).**

    Mitä käy, jos orjatietokoneille tulee samat ID:t jotenkin?
    
  - **Raportin tulee olla täsmällinen, toistettava sekä helppolukuinen (Karvinen 2006).**

    Mielestäni helppolukuisuus on raportin tärkein osa.

  ## b) Saltin asennus Linuxille (salt-minion)
  
  Asensin Saltin Salt Projectin nettisivujen (VMWare Inc) ohjeita hyödyntäen.

  Aloitin luomalla hakemiston julkisille avaimille komennolla:
  
  ```$ mkdir -p /etc/apt/keyrings```
  
  Tämän jälkeen latasin Salt Projectin julkisen avaimen, sekä Salt ohjelmalähteen määritystiedoston komennoilla:
    
  ```
  # 1. Julkinen avain:
  $ curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp  
  # 2. Ohjelmalähteen määritystiedosto:
  $ curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
  ```

  Seuraavaksi ajoin komennon: ```$ sudo apt update```

  Komento löysi päivityksiä Saltprojectilta, joten aikaisemmat komennot toimivat oikein:
  <img width="608" height="181" alt="image" src="https://github.com/user-attachments/assets/b5910187-099f-4321-a714-df9b6a407526" />

  Lopuksi asensin salt-minionin käyttäen komentoa:
  
  ```$ sudo apt-get install salt-minion```
  
  <img width="483" height="214" alt="image" src="https://github.com/user-attachments/assets/8a9f0ae1-32de-4450-874c-47d09e569209" />

  Ohjelman asennuksen jälkeen päätin vielä tarkistaa asentuiko Salt oikein.
  Tätä varten käytin komentoa:
  ``` $ sudo salt-call --version ```

  Sain vastauksen: ```salt-call 3007.8 (Chlorine)```, eli asennus sujui ongelmitta, sekä toimi.
  
## c) Viisi tärkeintä
  Tässä tehtävässä kokeilin Saltin tärkeimpiä tilafunktioita. Käytin ohjeina Tero Karvisen artikkelia Saltin komennoista (Karvinen 2018).

  ### pkg - Pakettien hallintamoduuli
 
  
  Kokeilin komentoa, jonka tarkoitus on tarkistaa ja asentaa tarvittaessa ohjelman. 
  
  Tässä tapauksessa "tree": 
  
  ```$ sudo salt-call --local -l info state.single pkg.installed tree ```

  <img width="597" height="443" alt="image" src="https://github.com/user-attachments/assets/a8c4f504-b6fe-48da-bb83-a444b2de9661" />

  - Komento asensi ohjelman onnistuneesti.
  - Komentoa uudelleenajaessa huomasin, että ohjelma tarkisti asennuksen ilman muutoksia.

  Seuraavaksi kokeilin samankaltaista komentoa, mutta asennuksen sijaan ohjelma poistetaan:
  
  ```$ sudo salt-call --local -l info state.single pkg.removed tree ```

  <img width="450" height="434" alt="image" src="https://github.com/user-attachments/assets/384cb6d0-07da-454d-bb25-34eb875dc635" />

  - Komento tarkisti onko ohjelma olemassa, sekä poisti sen tarvittaessa.
  - Komennon ajaminen uusiksi teki tarkistuksen ilman muutoksia, sillä ohjelmaa ei ollut enää olemassa.
  - Mieleen jäi kysymys: Onko Windowsin "bloatwarea" mahdollista pitää poistettuna tätä hyödyntäen?

### file - Tiedostojen hallinta


  - Tilafunktiolla hallinnoidaan tiedostoja ja hakemistoja järjestelmissä

    Ensimmäinen komento on file.managed:


    ``` $ sudo salt-call --local -l info state.single file.managed /tmp/hellojani ```

    <img width="604" height="516" alt="image" src="https://github.com/user-attachments/assets/02fcaec4-1aa2-4176-83e6-0041da948d65" />

  - Komento tarkisti, onko tiedosto olemassa ja loi tyhjän tiedoston, kun oli varmistanut sen olemattomuuden.
  - Tiedoston luonnin jälkeen tilafunktio muutti tuloksen true-muotoon.
  - Komennon uudelleenajo tulosti kommentin, että tiedosto /tmp/hellojani on olemassa, muutoksia ei tehty.

  Varmistin vielä itse tiedoston olemassaolon komennolla: ``` $ ls /tmp/hellojani ```

  <img width="478" height="62" alt="image" src="https://github.com/user-attachments/assets/bdd4911f-c8ef-430e-b348-c52e0499bd52" />

  - Tiedosto oli hakemistossa, joten komento toimi oikein.
  
 
  Seuraava komento tarkistaa, onko tiedosto olemassa valitulla sisällöllä, jos ei ole, se luo/lisää sinne valitun sisällön ja tiedoston:

  
  ```$ sudo salt-call --local -l info state.single file.managed /tmp/hellojani contents="foo" ```
 
  <img width="609" height="476" alt="image" src="https://github.com/user-attachments/assets/b9054e90-931c-466c-a2a8-f103d10cc9ac" />

  - Tilafunktio meni läpi, sekä päivitti aiemmin luodun tiedoston sisällöllä: "foo".
  - Uudelleenajoin komennon ja lokin mukaan tiedosto löytyi, eikä päivityksiä tarvinnut tehdä.

    Varmistin tämän komennolla: ``` $ cat /tmp/hellojani ```

    <img width="485" height="59" alt="image" src="https://github.com/user-attachments/assets/4938be6c-f7de-4de5-9aae-e60f01636084" />

    - Tiedosto löytyi, sekä oikea sisältö tulostettiin.


    Viimeinen komento poistaa tiedoston, jos se on olemassa:

    ```$ sudo salt-call --local -l info state.single file.absent /tmp/hellojani ```

    <img width="610" height="491" alt="image" src="https://github.com/user-attachments/assets/682c79db-7ba2-47f3-a890-4d848d427d8f" />

    - Komento onnistui ja teki yhden muutoksen (tässä tapauksessa poistaminen).
    - Lokin kommentiosiossa luki, mikä tiedosto poistettiin.
    - Uudelleenajossa ei tehty muutoksia, sillä tiedostoa ei ollut enään olemassa.

    Varmistin vielä tämänkin komennolla: ```$ ls /tmp/hellojani ```

    <img width="566" height="59" alt="image" src="https://github.com/user-attachments/assets/206aeca9-8755-40c9-a3bc-5396ddc804a3" />

    - Komento varmisti poiston, sillä kyseistä tiedostoa ei löydy /tmp hakemistosta.

  ### service - Palveluiden hallinta

  - Tämä tilafunktio vastaa järjestelmän palveluiden hallinnasta.

  Seuraavaksi testasin komentoa service.running:

  ```$ sudo salt-call --local -l info state.single service.running apache2 enable=True```

  <img width="522" height="330" alt="image" src="https://github.com/user-attachments/assets/d38e83e4-303a-48f1-a33a-374c4e4d20e4" />

  Komento epäonnistui, joten jouduin asentamaan Apache2 seuraavalla komennolla: ```$ sudo apt install apache2```

  Apache2 asennuksen jälkeen kokeilin service.running komentoa uudelleen:
  
  <img width="488" height="326" alt="image" src="https://github.com/user-attachments/assets/f7022275-e53b-45cd-b865-319590815282" />
  
  - Komento tarkisti, onko apache2 käynnissä.
  - Muutoksia ei tapahtunut, sillä apache oli valmiiksi päällä.

  Seuraavaksi kokeilin komentoa service.dead:
  
  ```$ sudo salt-call --local -l info state.single service.dead apache2 enable=False```

  <img width="557" height="387" alt="image" src="https://github.com/user-attachments/assets/f03e1009-144e-4714-96fa-266d5475107e" />

  - Asennus onnistui ja teki muutoksen.
  - service.dead sulki apache2, sekä otti sen pois käytöstä käynnistyksessä.


### user - Käyttäjien hallinta

  - Järjestelmän käyttäjien hallinnoimisessa käytetään useria.

    Kokeilin komentoa user.present:

    ```$ sudo salt-call --local -l info state.single user.present terote08```

    <img width="368" height="689" alt="image" src="https://github.com/user-attachments/assets/4d847cbf-c402-4561-beed-16dde386ffd9" />
    
    - Komento onnistui.
    - user.present tarkasti, onko käyttäjä "terote08" olemassa.
    - Käyttäjää ei ollut, joten user.present loi uuden.

    Sitten kokeilin komentoa user.absent:

    ```$ sudo salt-call --local -l info state.single user.absent terote08```
    
    <img width="360" height="417" alt="image" src="https://github.com/user-attachments/assets/5cc1829e-475c-4227-b0f9-52463f44780b" />

    - Komento toimi samalla periaattella, kuin file.absent.
    - user.absent tarkisti, onko käyttäjä olemassa ja poisti sen.
   

### cmd - Komentojen suorittaja

  - Tilanfunktio "cmd" voi ajaa skriptejä tai komentoja etänä tai paikallisesti hallituilla järjestelmillä.

Kokeilin komentoa cmd.run:

```$ sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo"```

  <img width="389" height="453" alt="image" src="https://github.com/user-attachments/assets/48419c54-1898-4dfe-ad13-944d619d6c5c" />

- Komento loi tiedoston /tmp/foo.
- Komento ei tee muutoksia, ellei niitä tarvita, tehden siitä idempotentin.

  Tarkistin vielä, tekeekö se muutoksia uudelleenajettaessa:
  
  <img width="316" height="324" alt="image" src="https://github.com/user-attachments/assets/4744006b-00a0-4da0-ad6c-849ceca21e37" />

  - Komento ei tehnyt muutoksia, joten se toimi.


## d) Idempotentti

  Idempotentti tarkoittaa sitä, että komento/tila voidaan ajaa useaan otteeseen ilman, että se aiheuttaa turhia muutoksia.
  Tämän huomasin monessa komennossa aikaisemmassa tehtävässä, kun komento ei tehnyt muutoksia uudelleenajossa.
  
  Jos sinun täytyy asentaa ohjelma isossa yrityksessä järjestelmille, joilla sitä ei ole, idempotentti komento
  asentaa ohjelman vain järjestelmiin, joilta se puuttuu.

  Esimerkkinä komento:
  
  ```$ sudo salt-call --local -l info state.single pkg.installed tree```

  <img width="498" height="531" alt="image" src="https://github.com/user-attachments/assets/c77e75f7-11ca-4392-81e2-ce08d374c26a" />


  - Tässä ***pkg.installed*** tarkistaa, onko tree asennettu ja tarvittaessa asentaa sen.
  - Toisella ajokerralla se tarkisti asennuksen, mutta ei kuitenkaan tehnyt muutoksia.
  - Komento on siis idempotentti.
  - Jos komento ei olisi idempotentti, se asentaisi treen aina uudelleen ja uudelleen.


# Lähteet

Karvinen, T. 2025. Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/

Karvinen, T. 2023. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/

Karvinen, T. 2018. Salt Quickstart. Luettavissa: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

Karvinen, T. 2006. Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/

VMWare Inc. s.a. Install Salt: Linux (DEB) – Salt install guide. Luettavissa: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html
