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

PermitRootLogin prohibit-password

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

PasswordAuthentication no

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
Käytin komentoa: ```$ sudo systemctl restart ssh```, jonka jälkeen tarkastin palvelun statuksen komennolla: ```$ sudo systemctl status ssh```.

<img width="976" height="366" alt="image" src="https://github.com/user-attachments/assets/457722ec-a9ea-4e85-82be-722500fdafaa" />

Sitten käynnistin orja-tietokoneeni, jolla kokeilin SSH-yhteyttä master-koneeseen.

Käytin komentoa: ```$ ssh -p 1337 jani@192.168.1.253```.

<img width="847" height="213" alt="image" src="https://github.com/user-attachments/assets/3846709c-c7ea-46ff-a9bc-a344deabcf0b" />
