# h1 - Viisikko

Tässä kotitehtävässä kerron tiivistetysti parista artikkelista ranskalaisin viivoin. Sitten asennan Linuxin ja sen sisälle Salt (salt-minion).

  ## x) Lue ja tiivistä

  - Saltin tärkeimmät tilafunktiot ovat: pkg, file, service, user ja cmd (Karvinen 2023).
    
  - Voit hallinnoida tuhansia tietokoneita käyttäen Salttia (Karvinen 2018). Mietinkin tietoturvasta kiinnostuneena, että käytetäänkö tätä paljon kyberhyökkäyksissä? Todennäköisesti.
    
  - Asentaessa onedir version Saltista, Salt asentaa myös oman paikallisen version Pythonista, sekä muut toimivuuden kannalta tarvittavat riippuvuudet (VMWare Inc).
    
  - Orjatietokoneen täytyy tietää, missä isäntä on. Orjatietokoneilla tulee myös olla uniikit tunnukset (Karvinen 2018).
    
  - Raportin tulee olla täsmällinen, toistettava sekä helppolukuinen (Karvinen 2006).

  ## b) Asenna Salt Linuxille (salt-minion)
  
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

  Ohjelman asennuksen jälkeen päätin vielä tarkistaa, asentuiko Salt oikein.
  Tätä varten käytin komentoa:
  ``` $ sudo salt-call --version ```

  Sain vastauksen: ```salt-call 3007.8 (Chlorine)```, eli asennus meni läpi.
  
## c) Viisi tärkeintä. Näytä Linuxissa esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset.
