# h4 - Toimiva Versio

Tämä on raportti kotitehtävään Tero Karvisen (2025) palvelinten hallinta kurssille.

-  Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: **VMWare Workstation Pro**

## x) - Lue ja tiivistä

### *Pro Git, 2ed: 1.3 Getting Started - What is Git?* (Chacon and Straub 2014)

- Git säilyttää dataa "tilannekuvina", joita se luo projektiin esim. ```$ git commit``` komennon yhteydessä.
- Lähes jokainen operaatio Gitissä on paikallinen, sillä projektien kaikki tieto on saatavilla paikallisesti.

Git-tiedostot voivat olla kolmessa eri tilassa:

- Modified: Tiedostoa on muokattu, mutta sitä ei ole lähetetty tietokantaan.
- Staged: Muokattu tiedosto ja tiedostoversio on "merkattu" lähetettäväksi seuraavaan tilannekuvaan.
- Committed: Tiedosto ja sen data on säilytettynä paikallisessa tietokannassasi.
  
### *Git add, commit, and push* (Foster, G s.a)

- Komento: ```$ git add``` siirtää "modified" tiedoston "staged" vaiheeseen.
- Komennon: ```$ git commit``` ajo siirtää edellisen ```$ git add```-komennon "staged" tiedostot paikalliseen tilannekuvaan.
- ```$ git pull```-komennolla voit "vetää" esimerkiksi jaetusta varastosta uusimman päivityksen, jotta tilanne ei sekoitu.
- ```$ git push```-komento siirtää paikalliset päivitykset verkkoon Gitin palvelimille.

### *Github - terokarvinen/suolax* (Karvinen 2024)

<img width="1297" height="580" alt="image" src="https://github.com/user-attachments/assets/0c181338-d77a-4505-93d7-af8cc0b37eda" />

<img width="719" height="402" alt="image" src="https://github.com/user-attachments/assets/886b2816-df89-4b46-ab4f-557234495f8d" />

- Tero on lisännyt tilan "favourite-package", joka asentaa tarvittaessa useita oletettavasti hänen lempiohjelmia Saltilla.

## a) Online

Tässä tehtävässä loin uuden GitHub varaston, johon tuli ```README.md```-tiedosto, sekä GNU lisenssi.

Aloitin avaamalla Githubin nettisivun oikeasta ylälaidasta palkin, josta valitsin "*New repository*". 

<img width="229" height="335" alt="image" src="https://github.com/user-attachments/assets/8410acc2-03e7-4070-b882-07e3e9617bbc" />

Sitten lisäsin tehtävän vaatimat tiedot kuvan mukaisesti, jonka jälkeen klikkasin kohtaa "*Create repository*":

<img width="779" height="767" alt="image" src="https://github.com/user-attachments/assets/cb3c28c0-61cd-4212-95ca-559354bcc727" />

## b) Dolly

Tässä tehtävässä kloonasin juuri luomani varaston omaan Linux-ympäristööni.

Tätä varten minun piti luoda SSH-avainpari, jonka lisäsin GitHubiin.

Loin avainparin Linuxilla komennolla: ```$ ssh-keygen -t rsa -b 4096 -C "jani.karjalainen@myy.haaga-helia.fi"```.

Katsoin luomani avaimen komennolla: ```$ cat ~/.ssh/id_rsa.pub```. 

Sitten kopioin avaimen GitHubiin kohtaan " Settings --> SSH and GPG keys --> Add new SSH key:

<img width="953" height="491" alt="image" src="https://github.com/user-attachments/assets/d5ae8fef-5953-4346-baa6-103b19dd9cd0" />

Lisäsin myös omat tietoni Gittiin Linuxilla komennoilla:

```
git config --global user.email "jani.karjalainen@myy.haaga-helia.fi"
git config --global user.name "Karjiss"
```

Avaimen luonnin jälkeen kloonaus koneelle voi alkaa.

Kloonasin varaston komennolla: ```$ git clone git@github.com:Karjiss/yellow-snow.git```.

<img width="843" height="156" alt="image" src="https://github.com/user-attachments/assets/18dc0bc2-e1f3-4506-bd18-dade07855489" />

Siirryin varastoon komennolla: ```$ cd gitprojut/yellow-snow```.

Tein vielä muutoksen "README.md"-tiedostoon nanolla. Lisäsin sinne rivin "Hello World!".

Seuraavaksi käytin komentoa: ```$ git add .```, sitten: ```$ git commit```.

- ```$ git commit```-komennon yhteydessä kirjoitin lokiin maininnan tehdyistä muutoksista.

<img width="607" height="102" alt="image" src="https://github.com/user-attachments/assets/d0fb2c12-d29d-446c-8bba-2deb515b3d1e" />

Seuraavaksi päivitin varaston varmuuden vuoksi komennolla: ```$ git pull```.

<img width="589" height="21" alt="image" src="https://github.com/user-attachments/assets/3c21ebed-63a7-46bb-884e-9c64663e54d6" />

- Varasto oli jo uusimmassa versiossa, joten pystyin jatkamaan huoletta.

Viimeiseksi käytin komentoa: ```$ git push```, joka lähettää muutokset verkkopalvelimelle.

<img width="594" height="182" alt="image" src="https://github.com/user-attachments/assets/d7f59424-9f08-4490-865c-2e2f47587ac3" />

Tarkastin muutokset vielä verkkoselaimen kautta:

<img width="904" height="406" alt="image" src="https://github.com/user-attachments/assets/34d3e48e-d480-4929-a3ff-d18e284fbdc8" />

- Muutos oli mennyt läpi ja näkyi verkossa myös.

## c) Doh!

Tässä tehtävässä tein virheellisen muutoksen varastoon, jotta pystyin kokeilemaan palautusta.

Tyhjensin suurimman osan GNU lisenssin tiedostosta, sekä muutin sanoja sen sisällä:

<img width="660" height="159" alt="image" src="https://github.com/user-attachments/assets/8bf4395f-125a-41a9-9fd1-410409011c1d" />

Sitten komento: ```$ git add .```.

Käytin komentoa: ```$ git reset --hard``` korjatakseni virheet palauttamalla tiedoston aiempaan versioon.

<img width="669" height="402" alt="image" src="https://github.com/user-attachments/assets/5531ef26-b923-428e-a718-712b81608590" />

- Muokattu lisenssi-tiedosto palautui alkuperäiseen versioonsa, joten palautuskomento toimi.


## d) Tukki 

Tässä tehtävässä tarkastelin oman varaston lokia, sekä omat tietoni lokitiedoissa.

Käytin komentoa: ```$ git log --patch```.

<img width="706" height="309" alt="image" src="https://github.com/user-attachments/assets/ce0514e7-6b15-4924-8399-6d7773712b1c" />

- Lokissa näkyi tekemäni muutos README.md -tiedostoon, jota olin muokannut edellisen tehtävän jälkeen.
- Käyttäjätietoni näkyvät jokaisessa itsetekemässäni commitissa, sekä ne ovat oikein.

## e) Suolattu rakki

Tässä tehtävässä ajoin Salt-tiloja omasta varastostani. Karvisen (2024) GitHub-varastoa tilojen luonnissa.

Loin srv/salt/ hakemistot varastooni. Loin myös tiloille hakemiston "apachejuoksu".

Siirryin luomaani hakemistoon, jossa käytin komentoa: ```$ micro init.sls```.

**init.sls:**

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

- Tämä ajaa 2 tilaa samaan aikaan.
  
Puskin tehdyt muutokset GitHubiin.

Kokeilin ajaa tilat komennolla: ```$ sudo salt-call --local --file-root srv/salt state.apply apachejuoksu```.

<img width="858" height="544" alt="image" src="https://github.com/user-attachments/assets/9a7442a3-a2c6-40db-b0da-68b6bc0d9787" />

- Tilat suorituivat onnistuneesti.
- Muutoksia ei tehty, sillä paketti oli jo asennettu, sekä se oli jo käynnissä.



# Lähdeluettelo

Chacon, S & Straub, B. 2014. What is Git? Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F

Foster, Greg. s.a. Graphite. Luettavissa: https://graphite.com/guides/git-add-commit-push

Karvinen, Tero. 2024. Github. Luettavissa: https://github.com/terokarvinen/suolax/
