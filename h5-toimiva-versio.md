<img width="765" height="763" alt="image" src="https://github.com/user-attachments/assets/ac044c44-5b3d-4e8c-98b0-845a120e05b5" /># h4 - Toimiva Versio

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
  
### *Git add, commit, and push* (Foster, G)

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

<img width="765" height="763" alt="image" src="https://github.com/user-attachments/assets/9b04425d-bfb6-4345-a7a9-aae77d1865dd" />

## b) Dolly

Tässä tehtävässä kloonasin juuri luomani varaston omaan Linux-ympäristööni.

Tätä varten minun piti luoda SSH-avainpari, jonka lisäsin GitHubiin.

Loin avainparin Linuxilla komennolla: ```$ ssh-keygen -t rsa -b 4096 -C "jani.karjalainen@myy.haaga-helia.fi"```

Katsoin luomani avaimen komennolla: ```$ cat ~/.ssh/id_rsa.pub```. 

Sitten kopioin avaimen GitHubiin kohtaan " Settings --> SSH and GPG keys --> Add new SSH key:

<img width="953" height="491" alt="image" src="https://github.com/user-attachments/assets/d5ae8fef-5953-4346-baa6-103b19dd9cd0" />


# Lähteet

https://graphite.com/guides/git-add-commit-push

https://github.com/terokarvinen/suolax/
