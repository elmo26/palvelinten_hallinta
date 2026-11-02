# h1 Hei Ansiblen maailma

## x) Lue ja tiivistä

### Tiivistelmä SSH public key - Login without password [(Karvinen 2026)](https://terokarvinen.com/ssh-public-key-login-without-password/)
* SSH julkisen avaimen käyttö helpottaa monien työkalujen, kuten gitin ja ansiblen käyttöä, sillä salasanaa ei tarvitse käyttää joka kerta kun haluaa tehdä jotakin,
* OpenSSH-server paketin asennus `sudo apt-get update && sudo apt-get install ssh`
* SSH daemon starttaa käynnistettäessä `sudo systemctl enable ssh` Käynnistä myös nyt `--now` flagillä
* Kokeile SSH toimivuus ottamalla yhteyden localhostille `ssh localhost`
* SSH avainten generointi `ssh-keygen`, kopioi julkinen avain hostille `ssh-copy-id host`

### Tiivistelmä Hello Ansible [(Karvinen 2026)](https://terokarvinen.com/hello-ansible/)
* Asenna Ansible (ja pari ylimääräistä työkalua) `sudo apt-get update && sudo apt-get install ansible micro bash-completion tree` 
* Tehdään kansio `ansible` konfiguraatiota varten ja sinne tiedosto `hosts.ini`, jonne lisätään kontrolloitavat hostit. localhost on hyvä testatessa.
* Jos ansible valittaa ajettaessa pythonista, lisätään hosts.ini tiedostoon python kääntäjän polku `/usr/bin/python3` kaikille koneille, ja ansible ei enää valita.
* Ansiblen konfiguraatiotiedostoon `ansible.cfg` voi lisätä oletukseksi `hosts.ini` tiedoston, jolloin sitä ei tarvitse kirjoittaa jokaisella ajokerralla.
* `Site.yml` tiedostoon lisätään roolit koneryhmittäin - eli mitkä roolit halutaan millekkin koneille.
* Rooleille oma kansio `roles`, jonka sisällä rooleille omat kansionsa. Esim `roles/apache2`
* tree-ohjelma on kätevä piirtämään tiedostorakenteen loogisesti terminaaliin. Sillä on hyvä aloittaa ongelmien etsiminen, kun joku ei toimi tai tiedosto x on unohtunut.

## a) Paikallista Tehty tehtävässä h0 oikeasti netissä, kohdassa a) Linkki: https://github.com/elmo26/palvelinten_hallinta/blob/main/h0_oikeasti_netissa.md

## b) Sshecrets
Olin tehnyt tämän tunnilla, mutta koska tehtävässä käskettiin asentamaan ssh demoni käsin, poistin sen ensin koneeltani. Pysäytin ssh demonin komennolla `sudo systemctl stop ssh` ja varmistin, että se on pois päältä katsomalla statuksen komennolla `systemctl status ssh`.
Poistin ssh-demonin komennolla `sudo apt remove openssh-server`. Tämän jälkeen asensin sen takaisin komennolla `sudo apt install openssh-server`.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/ccbfc3ed-45b8-4ef2-98c9-d7284d27d89b" />
Kuva 1. Ssh-demonin (serverin) asennus

Tämän jälkeen piti testata ssh:n toimivuus joten ajoin palvelun ylös komennolla `sudo systemctl start ssh` ja sen jälkeen otin ssh-yhteyden localhostille. 

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/e7b9be82-6d61-40d6-af51-1c5be6e7832f" />

Kuva 2. Sshd ylösajo ja ssh-yhteys localhostille.

## c) Pubkey
Kuten edellisessä kohdassa näkyy, pääsin sisään ilman salasanakirjautumista. Olin edellisellä tunnilla pistänyt julkisen avaimen kirjautumisen pystyyn. Seuraavaksi poistin tämän menetelmän, jotta saisin sen laitettua uudelleen pystyyn.

Poistin `authorized_keys` tiedoston käyttäjäni kotihakemistosta komennolla `rm authorized_keys`, jotta julkisen avaimen kirjautuminen ei olisi käytössä ja testasin sen toimimattomaksi. Kuten kuvassa näkyy, `ssh localhost` vaatii minulta salasanaa `authorized_keys` tiedoston poiston jälkeen.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/9c4e7151-9167-4e89-860b-4f88afd1d4ab" />

Kuva 3. Authorized_keys tiedoston poisto ja varmistus, että avaimella ei pääse sisään.

Tämän jälkeen ajoin komennon `ssh-copy-id localhost`, joka lisää authorized_keys tiedostoon julkisen avaimen. Osoitettu kohde tässä taas localhost. Tämän jälkeen ssh-yhteys onnistui taas ilman salasanaa komennolla `ssh localhost`.

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/6eb206da-04e1-40aa-8150-370208f0fb70" />

Kuva 4. Julkisen avaimen lisääminen kohteeseen, kirjautuminen avaimella.

## d) Hei Ansible

## e) Vapaaehtoinen bonus

## Lähteet

Karvinen, T 29.10.2026 oppitunti - palvelinten hallinta

Karvinen, T 2026 Palvelinten Hallinta h1 Hei Ansiblen maailma: https://terokarvinen.com/palvelinten-hallinta/#h1-hei-ansiblen-maailma Luettu 2.11.2026

Karvinen, T 2026 SSH public key - login without password: https://terokarvinen.com/ssh-public-key-login-without-password/ Luettu 2.11.2026

Karvinen, T 2026 Hello Ansible: https://terokarvinen.com/hello-ansible/ Luettu 2.11.2026
