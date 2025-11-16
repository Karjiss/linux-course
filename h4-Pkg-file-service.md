# h4 - Pkg-file-service

Tämä on raportti kotitehtävään Tero Karvisen (2025) palvelinten hallinta kurssille.

-  Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: **VMWare Workstation Pro** ja **Vagrant**

## x) - Lue ja tiivistä

### Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port (Karvinen 2018)

- "Package-file-service" tilalla voidaan automatisoida SSH-konfigurointi ja ylläpito.
- Voit hallita tuhansia ohjelmia tällä tilalla.
- Jos SSH vastaa porttiin "**8888**", tila toimii.

## a) SSHouto

Tässä tehtävässä käytin pkg-file-serviceä SSH asennukseen, sekä käyttöönottoon herra-orja arkkitehtuurissa. 

Aloitin tehtävän kokeilemalla manuaalisesti SSH:n asennusta.

### Käsin tehty versio

Syötin master-koneella komennon: ```$ sudo apt-get install ssh```.

<img width="708" height="281" alt="image" src="https://github.com/user-attachments/assets/512c53e7-1470-43f7-9778-0eb584249c3d" />

Avasin SSH:n konfigurointitiedoston komennolla: ```$ sudoedit /etc/ssh/sshd_config```.

Poistin kommenttimerkit niistä asetuksista, mitä tarvitsin, sekä lisäsin rivin "Port 1337" ja "Port 22".

**sshd_config**:

```
# SSH Konffaus

Port 1337

Port 22

AddressFamily any

ListenAddress 0.0.0.0

ListenAddress ::

HostKey /etc/ssh/ssh_host_rsa_key

HostKey /etc/ssh/ssh_host_ecdsa_key

HostKey /etc/ssh/ssh_host_ed25519_key

RekeyLimit default none

SyslogFacility AUTH

LogLevel INFO

LoginGraceTime 2m

PermitRootLogin no

StrictModes yes

MaxAuthTries 6

MaxSessions 10

PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2

AuthorizedPrincipalsFile none

AuthorizedKeysCommand none

AuthorizedKeysCommandUser nobody

HostbasedAuthentication no

IgnoreUserKnownHosts no

IgnoreRhosts yes

PasswordAuthentication yes

PermitEmptyPasswords no

ChallengeResponseAuthentication no

#KerberosAuthentication no

#KerberosOrLocalPasswd yes

#KerberosTicketCleanup yes

#KerberosGetAFSToken no

#GSSAPIAuthentication no

#GSSAPICleanupCredentials yes

#GSSAPIStrictAcceptorCheck yes

#GSSAPIKeyExchange no

UsePAM yes

AllowAgentForwarding yes

AllowTcpForwarding yes

GatewayPorts no

X11Forwarding yes

X11DisplayOffset 10

X11UseLocalhost yes

PermitTTY yes

PrintMotd no

PrintLastLog yes

TCPKeepAlive yes

PermitUserEnvironment no

Compression delayed

ClientAliveInterval 0

ClientAliveCountMax 3

PidFile /var/run/sshd.pid

MaxStartups 10:30:100

PermitTunnel no

ChrootDirectory none

VersionAddendum none

Banner none

AcceptEnv LANG LC_*

Subsystem       sftp    /usr/lib/openssh/sftp-server

Match User anoncvs
    
    X11Forwarding no
    
    AllowTcpForwarding no
    
    PermitTTY no
    
    ForceCommand cvs server

ClientAliveInterval 120

#UseDNS no

``` 

- Kopioin sshd_config tiedoston talteen kotihakemistooni.

Seuraavaksi käynnistin ssh-palvelimen uudestaan, jotta asetukset tulevat voimaan.
Käytin komentoa: ```$ sudo systemctl restart ssh```, jonka jälkeen tarkistin palvelun statuksen komennolla: ```$ sudo systemctl status ssh```.

<img width="976" height="366" alt="image" src="https://github.com/user-attachments/assets/457722ec-a9ea-4e85-82be-722500fdafaa" />

Sitten käynnistin orja-tietokoneeni, jolla kokeilin SSH-yhteyttä master-koneeseen.

Käytin komentoa: ```$ ssh -p 1337 jani@192.168.1.253```.

<img width="847" height="213" alt="image" src="https://github.com/user-attachments/assets/3846709c-c7ea-46ff-a9bc-a344deabcf0b" /> <img width="529" height="40" alt="image" src="https://github.com/user-attachments/assets/3c6740c1-6c93-45b5-afa9-231807587ab0" />


- Yhteys toimii, mutta julkisen avaimen puuttuminen hylkää yhteyden.

### Automatisoitu versio


<img width="824" height="561" alt="image" src="https://github.com/user-attachments/assets/e0d8d621-18ee-42ef-a1e2-0d69e7e72a9e" />

Seuraavaksi aloin luomaan Saltilla tilafunktiota, joka automatisoi koko operaation.

Loin tilalle oman hakemiston komennolla: ```$ mkdir -p /srv/salt/sshd_konffi```.

Lisäsin aikaisemmin kopioimani **sshd_config**-tiedoston kotihakemistostani samaan hakemistoon komennolla: ```cp sshd_config /srv/salt/sshd_konffi```.

Lisäsin **init.sls**-tiedoston "sshd_konffi" hakemistoon, sekä aloin muokkaamaan sitä komennolla: ```$ sudoedit /srv/salt/sshd_konffi/init.sls```.

**init.sls**:
```
openssh-server:
  pkg.installed
/etc/ssh/sshd_config:
  file.managed:
    - source: "salt://sshd_konffi/sshd_config"
sshd:
  service.running:
    -watch:
      - file: /etc/ssh/sshd_config
```

Kokeilin ajaa tilan komennolla: ```$ sudo salt '*' state.apply sshd_konffi```.

**slave**:

<img width="836" height="604" alt="image" src="https://github.com/user-attachments/assets/6431b6ef-1a10-4339-badc-73623e71e49e" />

<img width="478" height="295" alt="image" src="https://github.com/user-attachments/assets/5f78cf05-ca8c-49f8-ad47-25c4d7e08193" />

- Pkg.installed asensi openssh-serverin onnistuneesti.
- Tila muutti **sshd_config**-tiedostoa onnistuneesti.
- Tila käynnisti ssh.servicen uudelleen.

<img width="459" height="382" alt="image" src="https://github.com/user-attachments/assets/891db6ab-cd34-4a14-a519-2700ea2ab7b4" />

- Uudelleenajaessa huomaa idempotenssin, sillä tila tarkisti kaiken, mutta ei tehnyt muutoksia.

Tarkistin vielä orjakoneella **sshd_config**-tiedoston muutokset komennolla ```$ cat /etc/ssh/sshd_config```

<img width="521" height="511" alt="image" src="https://github.com/user-attachments/assets/a23138b8-dfce-4883-a082-168befe92af8" />

- Muutokset näkyvät täälläkin, joten tilan suoriutui onnistuneesti.

Kokeilin tahalleen rikkoa **sshd_config**-tiedoston orjakoneella.

Muokkasin **sshd_config**-tiedostoa komennolla: ```$ sudoedit /etc/ssh/sshd_config```.

Muokkasin paria riviä tiedoston alusta tavalla, jonka pitäisi rikkoa tiedosto:

<img width="628" height="174" alt="image" src="https://github.com/user-attachments/assets/4d572849-d7c7-4b7f-aa69-517267f60e5b" />

Ajoin master-koneella tilan uudestaan:

<img width="439" height="540" alt="image" src="https://github.com/user-attachments/assets/dd8a4b26-285d-426e-94b9-eae4b6b7faa3" />

- File.managed muokkasi **sshd_config**-tiedoston takaisin haluttuun muotoon.
- Service.running käynnisti daemonin uudestaa, kun muutoksia on tapahtunut.
- Tila toimii siis täydellisesti.

Sitten kokeilin vielä SSH-yhteyttä orja-koneeseen masterilta komennolla: ```$ ssh -p 1337 slave@192.168.1.33```

<img width="664" height="172" alt="image" src="https://github.com/user-attachments/assets/7120eaec-b922-4699-9212-ccff29d1a1cc" />

